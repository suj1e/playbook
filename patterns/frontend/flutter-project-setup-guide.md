# Flutter 跨端 App 项目落地指南（可直接开干）

> 定位：**跨端 App 主工程**（iOS / Android）
> 目标：**一套代码、长期维护、可规模化**
> 思维：**Flutter 原生前端，而不是 Web 套壳**

---

## 1. 技术栈总览（主流且稳）

### 核心

* **Flutter 3.x**
* **Dart 3**（强类型、非 JS）

### 状态 & 数据

* **状态管理**：Riverpod（首选）
* **网络请求**：Dio
* **路由**：go_router

### 工程 & 质量

* **Mock**：Dio Interceptor + 本地 mock 数据
* **JSON 解析**：freezed + json_serializable
* **Lint**：flutter_lints

---

## 2. 项目结构（Flutter 原生思路）

```txt
lib/
├── app/                    # 应用级配置
│   ├── app.dart
│   ├── router.dart
│   └── providers.dart
│
├── features/               # 功能单元（核心）⭐
│   ├── user/
│   │   ├── page.dart
│   │   ├── state.dart      # Riverpod
│   │   ├── api.dart        # 请求定义
│   │   ├── model.dart      # freezed model
│   │   ├── mock.dart       # mock 数据
│   │   └── ui/
│   └── order/
│
├── shared/                 # 无业务语义
│   ├── widgets/
│   ├── utils/
│   └── theme/
│
├── services/               # 基础能力
│   ├── http.dart
│   └── env.dart
│
└── main.dart
```

### 结构原则

* 功能是第一维度，不是页面 / widget
* Feature 内自闭环
* Widget 尽量无状态（Stateless）

---

## 3. 状态管理：为什么是 Riverpod

### 选择原因

* 无 Context 依赖
* 编译期安全
* 易测试、易拆分

### 基本范式

```dart
final userListProvider = FutureProvider<List<User>>((ref) async {
  return ref.read(userApiProvider).getUsers();
});
```

页面中：

```dart
class UserPage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final users = ref.watch(userListProvider);

    return users.when(
      loading: () => const CircularProgressIndicator(),
      error: (e, _) => Text(e.toString()),
      data: (data) => UserList(users: data),
    );
  }
}
```

---

## 4. Model & 类型（必须做）

```dart
@freezed
class User with _$User {
  const factory User({
    required String id,
    required String name,
  }) = _User;

  factory User.fromJson(Map<String, dynamic> json) =>
      _$UserFromJson(json);
}
```

* 禁止 Map 到处飞
* 所有接口返回必须建模

---

## 5. HTTP & Mock 动态切换

### Dio 统一封装

```dart
final dioProvider = Provider<Dio>((ref) {
  final dio = Dio(BaseOptions(baseUrl: Env.apiBase));
  dio.interceptors.add(LogInterceptor());
  dio.interceptors.add(MockInterceptor(ref));
  return dio;
});
```

---

### Mock Interceptor（核心）

```dart
class MockInterceptor extends Interceptor {
  final Ref ref;
  MockInterceptor(this.ref);

  @override
  void onRequest(RequestOptions options, RequestInterceptorHandler handler) {
    if (!Env.enableMock) return handler.next(options);

    if (options.path == '/users') {
      return handler.resolve(
        Response(
          requestOptions: options,
          data: mockUsers,
        ),
      );
    }

    handler.next(options);
  }
}
```

---

### 环境控制

```dart
class Env {
  static const enableMock = bool.fromEnvironment('ENABLE_MOCK');
  static const apiBase = String.fromEnvironment('API_BASE');
}
```

```bash
flutter run --dart-define=ENABLE_MOCK=true
```

---

## 6. 路由（go_router）

```dart
final router = GoRouter(
  routes: [
    GoRoute(path: '/login', builder: (_, __) => LoginPage()),
    GoRoute(path: '/users', builder: (_, __) => UserPage()),
  ],
);
```

* 路由即页面栈
* 权限控制写在 redirect

---

## 7. 生命周期 & App 状态

* WidgetsBindingObserver
* 前后台切换
* 网络重连

```dart
class AppLifecycle with WidgetsBindingObserver {
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    // resumed / paused
  }
}
```

---

## 8. 开发流程建议（非常重要）

1. 新建 feature
2. 先写 model
3. 写 mock
4. 接 Riverpod
5. 最后写 UI

---

## 9. 常见反模式（必踩坑预警）

❌ setState 管所有状态
❌ Widget 超过 300 行
❌ Map<String, dynamic> 满天飞
❌ 业务逻辑写在 build 里

---

## 10. 一句话总结

> **Flutter 项目成败，80% 决定于：状态模型 + 工程结构，而不是 UI。**

---

这份文档可直接作为：

* Flutter 项目 README
* 团队开发规范
* 新人上手指南

---
