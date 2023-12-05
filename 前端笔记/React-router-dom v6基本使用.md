## 快速开始

```jsx
// main.tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'
import 'tdesign-react/esm/style/index.js';
import {HashRouter} from "react-router-dom"; // 少量公共样式

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
    <React.StrictMode>
        <HashRouter>
            <App/>
        </HashRouter>
    </React.StrictMode>
)
```

```jsx
// App.tsx
import React, {Suspense} from "react";
import routes from "./router/router";
import {useRoutes} from "react-router-dom";

function App() {
    document.documentElement.removeAttribute('theme-mode');
    return (
        <Suspense fallback={
            <>
                lanjiazai
            </>
        }>
            {
                useRoutes(routes)
            }
        </Suspense>
    )
}

export default App
```

```tsx
//router.tsx
import {lazy, ReactNode, Suspense} from "react";

const lazyLoad = (children: ReactNode) => {
    return (
        <Suspense fallback={<>loading</>}>
            {children}
        </Suspense>
    )
}
const Login = lazy(() => import('../views/Login/Login'))
const routes = [
    {
        path: "/",
        element: lazyLoad(<Login/>),
    }
]

export default routes;
```

## React-router-v6路由懒加载

参考文章：[https://juejin.cn/post/7127896640572096525](https://juejin.cn/post/7127896640572096525)

> hook方式的路由懒加载


```jsx
// App.jsx
import './App.css'
import {useRoutes} from "react-router-dom";
import React, {Suspense} from "react";
import routes from "./router/router.jsx";
import "@arco-design/web-react/dist/css/arco.css";

function App() {
    return (
        <Suspense fallback={
            <>
                lanjiazai
            </>
        }>
            {
                useRoutes(routes)
            }
        </Suspense>
    )
}

export default App
```

```jsx
// router.jsx
import {lazy} from "react";
const OsMsg = lazy(() => import('../views/MsgManage/OsMsg/osMsg.jsx'))
const SortManage = lazy(() => import('../views/MediaLibrary/SortManage/SortManage.jsx'));
const MediaManage = lazy(() => import("../views/MediaLibrary/MediaManage/MediaManage.jsx"));
const EncyclopediaAnimals = lazy(() => import( "../views/EncyclopediaAnimals/EncyclopediaAnimals.jsx"))

const Login = lazy(() => import('../views/Login/Login.jsx'))
const Dashboard = lazy(() => import('../views/Dashboard/Dashboard.jsx'))
const Children = lazy(() => import('../views/Dashboard/children/Children.jsx'))
const UserManage = lazy(() => import('../views/UserManage/UserManage.jsx'))
const RoleManage = lazy(() => import("../views/RoleManage/RoleManage.jsx"))
const Setting = lazy(() => import('../views/Setting/Setting.jsx'))
const MenuManage = lazy(() => import('../views/MenuManage/MenuManage.jsx'))

const routes = [
    {
        path: '/',
        element: <Login/>
    },
    {
        path: '/dash',
        element: <Dashboard/>,
        children: [
            {
                //仪表盘
                path: 'dash_home',
                element: <Children/>
            },
            {
                //用户管理
                path: 'user_manage',
                element: <UserManage/>,
            }, {
                path: 'per_management',
                children: [
                    {
                        //菜单路由管理
                        path: 'menu_manage',
                        element: <MenuManage/>,
                    }, {
                        //角色管理
                        path: 'role_manage',
                        element: <RoleManage/>,
                    }
                ]
            },
            {
                path: 'setting',
                children: [
                    {
                        //系统基础配置
                        path: 'base',
                        element: <Setting/>,
                    }
                ]
            },
            {
                // 消息管理
                path: 'msg_manage',
                children: [
                    {
                        path: 'os_msg',
                        element: <OsMsg/>
                    }
                ]
            }, {
                // 媒体库
                path: 'media_library',
                children: [
                    {
                        path: 'sort_management',
                        element: <SortManage/>
                    }, {
                        path: 'media_manage',
                        element: <MediaManage/>
                    }
                ]
            }, {
                // 媒体库
                path: 'ency_animals',
                element: <EncyclopediaAnimals/>
            }
        ]
    }
];

export default routes;
```

#### 避免闪屏

-  公共的布局模块不进行懒加载，而是正常引入（预加载） 
-  suspense 
   - Suspense 让组件遇到异步操作时进入“悬停”状态，等异步操作有结果时再回归正常状态。
   - 异步操作分为两类：1. 异步加载代码 2. 异步加载数据
   - 使用优化，代码如下：

```jsx
import AppLayout from '../AppLayout' // 此处不进行懒加载
const Home = lazy(() => import('../pages/Home')) // 此处需要懒加载
const lazyLoad = (children: ReactNode): ReactNode => {
    return <Suspense fallback={<>loading</>}>
        {children}
    </Suspense>
}
const router: RouteObject[] =[
    {
        path: '/',
        element: <AppLayout/>, // 此处不进行懒加载
        children: [
            {
                path: 'home',
                element: lazyLoad(<Home /> // 此处需要懒加载
            }
        ]
    }
]
```
