# React 前端项目落地指南（含 Mock 动态切换）

> 目标：**直接拿去开新项目**
> 取向：**纯前端思维、可长期维护、开发体验优先**
> 适用：中大型 Web 项目 / 平台型前端

---

## 1. 技术栈总览（直接定死）

### 核心框架

* **React 18**
* **TypeScript（strict）**
* **Vite**

### 状态 & 数据

* **路由**：React Router v6
* **远程数据**：TanStack Query
* **共享状态**：Zustand（少量、克制）

### UI & 样式

* **样式**：Tailwind CSS
* **UI 组件**：shadcn/ui 或 Ant Design
* **图标**：lucide-react

### Mock & 工程

* **Mock 框架**：MSW（Mock Service Worker）⭐
* ESLint + Prettier
* pnpm

---

## 2. 项目结构（前端原生）

```txt
src/
├── app/                    # 应用级配置
│   ├── App.tsx
│   ├── router.tsx
│   ├── providers.tsx
│   └── mock.ts             # mock 开关 ⭐
│
├── features/               # 功能单元（核心）⭐
│   ├── user/
│   │   ├── page.tsx        # 页面（可脏）
│   │   ├── api.ts          # API 定义
│   │   ├── hooks.ts        # 业务 Hook
│   │   ├── state.ts        # 可选（Zustand）
│   │   ├── mock.ts         # 本功能 mock
│   │   └── ui/             # 纯 UI 组件
│   └── order/
│
├── shared/                 # 无业务语义
│   ├── ui/
│   ├── hooks/
│   └── utils/
│
├── mocks/                  # MSW 聚合
│   ├── handlers.ts
│   └── browser.ts
│
└── main.tsx
```

### 结构原则

* 功能 = 页面 + 状态 + 交互
* 页面只做组合，不写复杂逻辑
* 逻辑沉淀在 Hook
* UI 组件必须是"哑组件"

---

## 3. 前端核心开发范式（必须遵守）

### 页面（Page）

* 负责：数据组合、状态分发
* 不负责：业务实现

```tsx
export function UserPage() {
  const { data, isLoading } = useUsers()
  if (isLoading) return <Loading />
  return <UserTable users={data} />
}
```

---

### Hook（业务最小单元）

```ts
export function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: getUsers,
  })
}
```

---

### UI 组件（纯展示）

```tsx
function UserTable({ users }: Props) {
  return (...)
}
```

---

## 4. HTTP & API 规范

```ts
// services/http.ts
export const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE,
  timeout: 10000,
})
```

### 规范

* 页面 **禁止** 直接请求接口
* API 必须有返回类型
* 不写 any

---

## 5. Mock 方案（重点）

### 为什么选 MSW

* 在 **网络层** mock
* 不污染业务代码
* mock / 真后端 一键切换

---

### Mock 定义（按功能）

```ts
// features/user/mock.ts
import { http, HttpResponse } from 'msw'

export const userHandlers = [
  http.get('/users', () =>
    HttpResponse.json([
      { id: '1', name: 'Alice' },
      { id: '2', name: 'Bob' },
    ])
  ),
]
```

---

### Mock 聚合

```ts
// mocks/handlers.ts
export const handlers = [
  ...userHandlers,
]
```

---

### 动态切换 Mock / 后端

```ts
// app/mock.ts
export async function enableMocking() {
  if (import.meta.env.VITE_ENABLE_MOCK !== 'true') return
  const { worker } = await import('@/mocks/browser')
  return worker.start()
}
```

```ts
// main.tsx
enableMocking().then(() => renderApp())
```

```env
# .env.local
VITE_ENABLE_MOCK=true
```

---

## 6. 状态管理铁律

### 状态优先级

1. URL
2. React Query
3. Component State
4. Zustand（迫不得已）

❌ Redux First
❌ 全局 Store 设计

---

## 7. 工程规范（最小集）

* ESLint + Prettier
* 禁止 any
* Hook 必须以 use 开头
* 一个功能一个 feature 目录

---

## 8. 常见反模式（CR 必打回）

❌ useEffect 写业务流程
❌ 页面直接调 axios
❌ 组件里发请求
❌ mock 混在业务代码里

---

## 9. 开发流程建议

1. 新建 feature 目录
2. 先写 mock
3. 再写 Hook
4. 最后写页面和 UI

> **先能跑，再对接后端**

---

## 10. 一句话总总结

> **好前端项目 = 状态清晰、逻辑集中、UI 干净、mock 可随时开关。**

---

这份文档可直接作为：

* 项目 README
* 技术选型说明
* 团队开发规范

---
