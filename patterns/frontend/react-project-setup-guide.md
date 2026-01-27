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

### HTTP 客户端配置

```ts
// services/http.ts
export const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE,
  timeout: 10000,
})

// 请求拦截器：统一添加 token
http.interceptors.request.use((config) => {
  const token = getToken()
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

// 响应拦截器：统一处理错误
http.interceptors.response.use(
  (response) => response.data,
  (error) => {
    // 401: 跳转登录
    if (error.response?.status === 401) {
      window.location.href = '/login'
    }
    // 统一错误提示
    const message = error.response?.data?.message || '请求失败'
    toast.error(message)
    return Promise.reject(error)
  }
)
```

### API 定义规范

```ts
// features/user/api.ts
import { http } from '@/services/http'
import { z } from 'zod'

// 类型定义（用 Zod 验证）
export const UserSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
})

export type User = z.infer<typeof UserSchema>

// API 函数（有类型、有错误处理）
export async function getUsers(): Promise<User[]> {
  const data = await http.get<User[]>('/users')
  return z.array(UserSchema).parse(data)
}

export async function updateUser(id: string, user: Partial<User>): Promise<User> {
  const data = await http.put<User>(`/users/${id}`, user)
  return UserSchema.parse(data)
}
```

### 规范

* 页面 **禁止** 直接请求接口
* API 必须有返回类型
* 不写 any
* 用 Zod 验证后端返回
* token、错误在拦截器统一处理

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

## 6. 错误处理（必须）

### 6.1 全局错误边界

```tsx
// app/ErrorBoundary.tsx
export class ErrorBoundary extends Component {
  state = { hasError: false, error: null }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error }
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error('Error caught:', error, errorInfo)
    // 上报错误监控
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback error={this.state.error} />
    }
    return this.props.children
  }
}

// main.tsx
<ErrorBoundary>
  <App />
</ErrorBoundary>
```

### 6.2 TanStack Query 错误处理

```tsx
// features/user/hooks.ts
export function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: getUsers,
    // 错误重试
    retry: (failureCount, error) => {
      if (error.status === 401) return false // 401 不重试
      return failureCount < 3
    },
    // 错误回调
    onError: (error) => {
      toast.error(`加载失败: ${error.message}`)
    },
  })
}

// 页面中使用
export function UserPage() {
  const { data, isLoading, error } = useUsers()

  if (error) {
    return <ErrorPage error={error} />
  }
  // ...
}
```

### 6.3 错误处理铁律

* 必须有全局 ErrorBoundary
* API 错误在拦截器统一处理
* Query 错误在 Hook 层处理
* 不要吞掉错误

---

## 7. 性能优化（避免常见坑）

### 7.1 代码分割 & 懒加载

```tsx
// app/router.tsx
import { lazy, Suspense } from 'react'

const UserPage = lazy(() => import('@/features/user/page'))
const OrderPage = lazy(() => import('@/features/order/page'))

export function AppRouter() {
  return (
    <Suspense fallback={<PageSkeleton />}>
      <Routes>
        <Route path="/users" element={<UserPage />} />
        <Route path="/orders" element={<OrderPage />} />
      </Routes>
    </Suspense>
  )
}
```

### 7.2 memo 陷阱（必须注意）

```tsx
// ❌ 错误：memo 无效（props 每次都是新对象）
<User data={{ id: 1, name: 'Alice' }} />

// ✅ 正确：稳定引用
const userData = useMemo(() => ({ id: 1, name: 'Alice' }), [])
<User data={userData} />

// ❌ 错误：每次渲染创建新函数
<Button onClick={() => handleClick(id)} />

// ✅ 正确：稳定引用
const handleClick = useCallback(() => {
  // ...
}, [id])
<Button onClick={handleClick} />
```

### 7.3 性能检查清单

* 路由级代码分割（必须）
* 大组件用 React.memo（配合稳定引用）
* useCallback/useMemo 只在需要时用
* 避免不必要的 state 更新
* 列表渲染必须有 key
* 避免在 render 中创建对象/函数

---

## 8. 环境变量管理（类型安全）

### 8.1 定义环境变量类型

```ts
// app/env.ts
import { z } from 'zod'

const envSchema = z.object({
  VITE_API_BASE: z.string().url(),
  VITE_ENABLE_MOCK: z.enum(['true', 'false']).transform(v => v === 'true'),
  VITE_APP_TITLE: z.string().default('My App'),
})

export const env = envSchema.parse(import.meta.env)

// 使用
const apiBase = env.VITE_API_BASE // 类型安全
```

### 8.2 .env 文件

```env
# .env.development
VITE_API_BASE=http://localhost:3000/api
VITE_ENABLE_MOCK=true

# .env.production
VITE_API_BASE=https://api.example.com
VITE_ENABLE_MOCK=false
```

### 铁律

* 环境变量必须用 Zod 验证
* 敏感信息（密钥、密码）禁止放前端
* 前端变量必须以 `VITE_` 开头

---

## 9. 安全性（必须注意）

### 9.1 XSS 防护

```tsx
// ❌ 危险：直接渲染 HTML
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// ✅ 安全：React 自动转义
<div>{userInput}</div>

// 必须用 HTML 时，先消毒
import DOMPurify from 'dompurify'
const clean = DOMPurify.sanitize(userInput)
<div dangerouslySetInnerHTML={{ __html: clean }} />
```

### 9.2 Token 管理

```ts
// ❌ 危险：token 存 localStorage
localStorage.setItem('token', token)

// ✅ 安全：token 存 httpOnly cookie（后端设置）
// 前端只需发送请求，cookie 自动携带

// 如果必须前端存，用 sessionStorage + 短期过期
sessionStorage.setItem('token', token)
```

### 9.3 安全检查清单

* 禁止 `dangerouslySetInnerHTML` 除非消毒
* Token 优先用 httpOnly cookie
* 禁止在 URL 传敏感信息
* API 调用必须用 HTTPS（生产）
* CSRF token 由后端处理

---

## 10. 路由实战

### 10.1 路由守卫

```tsx
// app/router.tsx
import { Navigate } from 'react-router-dom'

function ProtectedRoute({ children }: { children: ReactNode }) {
  const { isAuthenticated } = useAuth()

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />
  }

  return children
}

// 使用
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <DashboardPage />
    </ProtectedRoute>
  }
/>
```

### 10.2 嵌套路由 & 加载状态

```tsx
export function AppRouter() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<HomePage />} />
        <Route path="users">
          <Route index element={<UserListPage />} />
          <Route path=":id" element={<UserDetailPage />} />
        </Route>
      </Route>
      <Route path="*" element={<NotFoundPage />} />
    </Routes>
  )
}
```

### 10.3 路由级数据加载

```tsx
// 配合 TanStack Router 或 React Router v6.4+ 的 loader
export function userLoader() {
  return getUsers()
}

<Route path="/users" element={<UserPage />} loader={userLoader} />
```

---

## 11. 副作用管理（useEffect 正确用法）

### 11.1 什么时候用 useEffect

* 外部系统同步（DOM、第三方库）
* 订阅/事件监听
* 日志上报
* **不是**用来触发业务流程

### 11.2 常见错误模式

```tsx
// ❌ 错误：用 useEffect 做数据转换
const [processed, setProcessed] = useState([])
useEffect(() => {
  setProcessed(data.map(transform))
}, [data])

// ✅ 正确：直接在渲染时计算
const processed = useMemo(() => data.map(transform), [data])

// ❌ 错误：用 useEffect 触发 API
useEffect(() => {
  fetchUser(id)
}, [id])

// ✅ 正确：用 TanStack Query
const { data } = useUser(id)

// ✅ 正确的 useEffect 用法：订阅
useEffect(() => {
  const ws = new WebSocket(url)
  ws.onmessage = (e) => handleMessage(e.data)
  return () => ws.close()
}, [url])
```

### 11.3 useEffect 检查清单

* 依赖数组完整吗？（eslint-plugin-react-hooks）
* 是否可以用 useMemo 替代？
* 是否可以用事件处理器替代？
* cleanup 函数写了吗？

---

## 12. 状态管理铁律

### 状态优先级

1. URL
2. React Query
3. Component State
4. Zustand（迫不得已）

❌ Redux First
❌ 全局 Store 设计

---

## 13. 工程规范（最小集）

* ESLint + Prettier
* 禁止 any
* Hook 必须以 use 开头
* 一个功能一个 feature 目录

---

## 14. 常见反模式（CR 必打回）

❌ useEffect 写业务流程（用 Query/Hook 替代）
❌ 页面直接调 axios（通过 API 层）
❌ 组件里发请求（抽到 Hook）
❌ mock 混在业务代码里（用 MSW）
❌ memo 包裹但不控制 props 引用（无效优化）
❌ 用 useState 保存派生数据（用 useMemo）
❌ dangerouslySetInnerHTML 不消毒（XSS 风险）

---

## 15. 开发流程建议

1. 新建 feature 目录
2. 先写 mock
3. 再写 Hook
4. 最后写页面和 UI

> **先能跑，再对接后端**

---

## 16. 项目检查清单（上线前必查）

### 16.1 功能检查
- [ ] 所有 API 有类型定义
- [ ] 所有 API 用 Zod 验证
- [ ] 全局 ErrorBoundary 已配置
- [ ] 路由守卫已配置
- [ ] Mock 开关可正常切换

### 16.2 性能检查
- [ ] 路由级懒加载
- [ ] 大组件已优化
- [ ] 无明显渲染性能问题

### 16.3 安全检查
- [ ] 无 XSS 风险
- [ ] Token 存储安全
- [ ] 敏感信息不在前端

### 16.4 代码质量
- [ ] 无 ESLint 错误
- [ ] 无 any 类型
- [ ] 无 console.log残留

---

## 17. 一句话总总结

> **好前端项目 = 状态清晰、逻辑集中、UI 干净、mock 可随时开关。**

---

这份文档可直接作为：

* 项目 README
* 技术选型说明
* 团队开发规范

---
