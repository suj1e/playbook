# 前端设计模式

> 构建 React/Vue/Angular 应用的最佳实践

---

## 目录

- [1. 组件模式](#1-组件模式)
- [2. 状态管理模式](#2-状态管理模式)
- [3. 数据获取模式](#3-数据获取模式)
- [4. 组合模式](#4-组合模式)
- [5. 容器模式](#5-容器模式)
- [6. Hooks 模式](#6-hooks-模式)
- [7. 性能优化模式](#7-性能优化模式)
- [8. 安全模式](#8-安全模式)

---

## 1. 组件模式

### 1.1 展示组件 (Presentational Components)

**职责：** 纯 UI 展示，不包含业务逻辑。

```tsx
// ❌ 错误：包含业务逻辑
const UserList = ({ users }) => {
  const [sortedUsers, setSortedUsers] = useState(users);

  useEffect(() => {
    setSortedUsers(users.sort((a, b) => a.name.localeCompare(b.name)));
  }, [users]);

  return <ul>{sortedUsers.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
};

// ✅ 正确：纯展示组件
const UserList = ({ users }) => {
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
};
```

---

### 1.2 容器组件 (Container Components)

**职责：** 数据获取、状态管理、事件处理。

```tsx
// 容器组件
export const UserListContainer = () => {
  const { data, isLoading, error } = useUsers();

  if (isLoading) return <Loading />;
  if (error) return <Error message={error.message} />;
  if (!data) return <Empty />;

  return <UserList users={data} />;
};

// 展示组件
const UserList = ({ users }) => {
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
};
```

---

### 1.3 高阶组件 (HOC)

**意图：** 组件复用逻辑，但 React Hooks 已基本替代。

```tsx
// HOC 示例（已过时，建议用 Hooks）
function withLoading<P extends object>(
  Component<P>
) {
  return function WithLoading({ isLoading, ...props }) {
    if (isLoading) {
      return <Loading />;
    }
    return <Component {...props} />;
  };
}

// 现代 Hooks 替代方案
function useLoading<T>(isLoading: boolean, data: T | null) {
  if (isLoading) return <Loading />;
  if (!data) return <Empty />;
  return data;
}
```

---

### 1.4 Render Props

**意图：** 通过 props 传递组件内部状态，实现组件复用。

```tsx
// 组件通过 props 暴露内部状态
interface TabsProps {
  activeTab: string;
  onTabChange: (tab: string) => void;
}

export const Tabs: React.FC<TabsProps> = ({ activeTab, onTabChange }) => {
  return (
    <div>
      <TabBar activeTab={activeTab} onTabChange={onTabChange}>
        <Tab id="home">Home</Tab>
        <Tab id="profile">Profile</Tab>
        <Tab id="settings">Settings</Tab>
      </TabBar>
      <TabContent>
        {activeTab === 'home' && <HomeContent />}
        {activeTab === 'profile' && <ProfileContent />}
      </TabContent>
    </div>
  );
};
```

---

## 2. 状态管理模式

### 2.1 Provider 模式

**意图：** 跨组件状态共享。

```tsx
// Context 定义
type User = {
  id: string;
  name: string;
  email: string;
};

type UserContextType = {
  user: User | null;
  login: (user: User) => void;
  logout: () => void;
};

const UserContext = createContext<UserContextType | null>(null);

// Provider
export const UserProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = (userData: User) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {children}
    </UserContext.Provider>
  );
};

// 使用
export const useUser = () => {
  const context = useContext(UserContext);
  if (!context) throw new Error('useUser must be used within UserProvider');
  return context;
};
```

---

### 2.2 Redux / Zustand 模式

**Zustand 使用：**

```typescript
// store/user.ts
import create from 'zustand';

interface User {
  id: string;
  name: string;
}

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));

// 组件中使用
const { user, setUser } = useUserStore();
```

---

### 2.3 状态优先级

```
1. URL 状态
2. 服务器状态（React Query）
3. 组件状态（useState）
4. 全局状态（Zustand/Redux）
```

---

## 3. 数据获取模式

### 3.1 SWR 模式

**特点：** 自动缓存、自动重新获取、自动重试。

```tsx
// SWR Hook
import useSWR from 'swr';

function useUser(id: string) {
  const { data, error, isLoading } = useSWR(
    `/api/users/${id}`,
    fetcher => fetch(`/api/users/${id}`).then(r => r.json())
  );

  return { user: data, error, isLoading };
}
```

---

### 3.2 React Query 模式

**特点：** 强大、可扩展、后端管理。

```tsx
import { useQuery } from '@tanstack/react-query';

function useUser(id: string) {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => fetchUser(id),
    staleTime: 5 * 60 * 1000, // 5 分钟
    cacheTime: 10 * 60 * 1000, // 10 分钟
    retry: 3,
  });
}
```

---

### 3.3 数据获取最佳实践

| 模式 | 优点 | 缺点 | 适用场景 |
|------|------|------|----------|
| SWR | 简单、自动 | 功能较少 | 小型项目 |
| React Query | 功能强大、社区活跃 | 配置复杂 | 中大型项目 |
| Zustand Query | 与状态管理集成 | 需要手动配置 | 简单状态+查询 |

---

## 4. 组合模式

### 4.1 Compound Components

**意图：** 通过组合多个小组件构建复杂 UI。

```tsx
// 基础组件
const Button = ({ children, onClick, variant = 'primary' }) => (
  <button className={`btn btn-${variant}`} onClick={onClick}>
    {children}
  </button>
);

const Input = ({ label, ...props }) => (
  <div className="form-group">
    <label>{label}</label>
    <input {...props} />
  </div>
);

// 组合组件
const Form = ({ onSubmit, children }) => (
  <form onSubmit={onSubmit}>
    {children}
  </form>
);

// 使用
<Form onSubmit={handleSubmit}>
  <Input label="用户名" />
  <Input label="密码" type="password" />
  <Button type="submit">提交</Button>
</Form>
```

---

### 4.2 Slot 模式

**意图：** 组件内容的插槽占位符。

```tsx
// Card 组件
interface CardProps {
  title: string;
  children: ReactNode;
  actions?: ReactNode;
}

export const Card: React.FC<CardProps> = ({ title, children, actions }) => (
  <div className="card">
    <div className="card-header">
      <h3>{title}</h3>
    </div>
    <div className="card-body">
      {children}
    </div>
    {actions && (
      <div className="card-footer">
        {actions}
      </div>
    )}
  </div>
);

// 使用
<Card
  title="用户信息"
  actions={<Button>编辑</Button>}
>
  >
    <p>用户内容</p>
  </Card>
```

---

## 5. 容器模式

### 5.1 布局组件

**意图：** 提供页面布局框架。

```tsx
// DashboardLayout 布局组件
export const DashboardLayout: React.FC<{ children: ReactNode }> = ({ children }) => {
  return (
    <div className="dashboard-layout">
      <Sidebar />
      <Header />
      <main className="main-content">
        {children}
      </main>
    </div>
  );
};

// 使用
<DashboardLayout>
  <UserList />
</DashboardLayout>
```

---

### 5.2 页面容器模式

```tsx
// 页面容器
export const PageContainer: React.FC<{
  title: string;
  actions?: ReactNode;
}> = ({ title, actions, children }) => {
  return (
    <div className="page-container">
      <div className="page-header">
        <h1>{title}</h1>
        {actions && <div className="page-actions">{actions}</div>}
      </div>
      <div className="page-content">
        {children}
      </div>
    </div>
  );
};
```

---

## 6. Hooks 模式

### 6.1 自定义 Hooks

**意图：** 复用组件逻辑。

```tsx
// useLocalStorage Hook
function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : initialValue;
  });

  const setValue = useCallback(
    (value: T) => {
      setStoredValue(value);
      localStorage.setItem(key, JSON.stringify(value));
    },
    [key]
  );

  return [storedValue, setValue];
}

// 使用
const [user, setUser] = useLocalStorage('user', null);
```

---

### 6.2 useEffect 模式

**使用场景：**
- 副作用管理（订阅、事件监听）
- 外部系统同步（DOM、第三方库）
- 日志上报

**反模式：**
- ❌ 用 useEffect 做数据转换（用 useMemo）
- ❌ 用 useEffect 触发 API（用 React Query）
- ❌ 用 useEffect 同步状态（直接在组件中同步）

---

## 7. 性能优化模式

### 7.1 虚拟化长列表

```tsx
import { FixedSizeList } from 'react-window';

function UserList({ users }: { users: User[] }) {
  const Row = ({ index, style }: { index: number; style: React.CSSProperties }) => (
    <div style={style}>User: {users[index].name}</div>
  );

  return (
    <FixedSizeList
      height={600}
      itemCount={users.length}
      itemSize={50}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

---

### 7.2 React.memo 优化

```tsx
// ❌ 错误：memo 无效（props 每次都是新对象）
<User data={{ id: 1, name: 'Alice' }} />

// ✅ 正确：稳定引用
const userData = useMemo(() => ({ id: 1, name: 'Alice' }), []);

// 子组件
const User = React.memo(({ data }) => {
  return <div>{data.name}</div>;
});
```

---

### 7.3 useCallback 优化

```tsx
const handleClick = useCallback(() => {
    doSomething(id);
}, [id]);  // 依赖项
```

---

## 8. 安全模式

### 8.1 XSS 防护

```tsx
// ❌ 危险：直接渲染 HTML
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// ✅ 安全：React 自动转义
<div>{userInput}</div>

// 必须用 HTML 时，先消毒
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(userInput);
<div dangerouslySetInnerHTML={{ __html: clean }} />
```

---

### 8.2 CSRF 防护

```tsx
// 在请求头中携带 CSRF Token
const headers = {
  'X-CSRF-Token': getCsrfToken(),
};

fetch('/api/data', { headers, credentials: 'include' });
```

---

### 8.3 敏感数据保护

```typescript
// ❌ 危险：Token 存 localStorage
localStorage.setItem('token', token);

// ✅ 安全：Token 存 httpOnly Cookie（后端设置）
// 前端只需发送请求，cookie 自动携带

// 如果必须前端存，用 sessionStorage + 短期过期
sessionStorage.setItem('token', token);
```

---

## 速查表

### 组件模式
```tsx
// 展示组件
const Component = ({ data }) => <div>{data}</div>;

// 容器组件
const Container = () => {
  const { data } = useData();
  return data ? <Component data={data} /> : null;
};

// 高阶组件（已过时，用 Hooks 替代）
```

### 状态管理
```tsx
// Context + useContext
const Context = createContext(null);

// Zustand
const useStore = create((set) => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
});
```

### 数据获取
```tsx
// React Query
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
});

// SWR
const { data } = useSWR('/api/users');
```

---

## 相关文档

- [React 项目指南](../frontend/react-project-setup-guide.md)
- [Flutter 项目指南](../frontend/flutter-project-setup-guide.md)
- [组件库设计](../frontend/component-library.md)
