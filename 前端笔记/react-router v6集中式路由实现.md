参考文档：[点击查看](https://reactrouter.com/en/v6.3.0/api#useroutes)

## 安装

```bash
pnpm add react-router-dom@6
```

## 路由实现

```jsx
//入口：App.js
import { useRoutes } from 'react-router-dom';
import Routes from './router/router';
function App() {
  return useRoutes(Routes)    //Focus
}

export default App;
```

```jsx
//routes
import About from '../views/About';
import Home from '../views/Home';

const Routes = [
  {
    path: '/',
    element: <Home />,
  },
  {
    path: 'about',
    element: <About />,
  },
];

export default Routes;
```

## 路由函数跳转

```jsx
import React from 'react';
import { useNavigate } from 'react-router-dom';

function Home() {
    const navigate = useNavigate()   //Focus
    const btn = () =>{
        navigate('about')
    }
  return (
    <>
      <div>Home</div>
      <button onClick={btn}>to about</button>
    </>
  );
}

export default Home;
```

> 以上代码在`create-react-app`脚手架中没问题，在vite中需要调整`main.jsx`代码，引用`BrowserRouter` 标签。并且routes分离的话，文件要为`jsx` 格式


```jsx
//main.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'
import {BrowserRouter} from "react-router-dom";

ReactDOM.createRoot(document.getElementById('root')).render(
    <BrowserRouter>
        <App/>
    </BrowserRouter>
)
```

## 嵌套路由

```jsx
//routes定义
const routes = [
{
        path: '/dashboard',
        element: <Dashboard/>,
        children: [
            {
                path: 'test',
                element: <Test/>
            }
        ]
    }
]
```

在子路由展示的地方放上：`Outlet` 即可，需引入： `import {Outlet} from "react-router-dom";`

### 默认渲染二级嵌套路由

> 在`children` 中需要默认渲染的路由中加入：`index: true` 即可


```jsx
//routes
import Test from "../views/Dashboard/Test/Test.jsx";

const routes = [
    {
        path: '/dashboard',
        element: <Dashboard/>,
        children: [
            {
                index: true,    //重点
                element: <Test/>
            }
        ]
    }
];

export default routes;
```
