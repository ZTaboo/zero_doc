```jsx
//父组件
import React, {useRef} from 'react';
import HomeChild from "../component/HomeChild";

function Home() {
    const childRef = useRef()
    const btn = () => {
        let ref = childRef.current.getCon()
        console.log(`ref:${ref}`)
    }
    return (
        <>
            <div>Home</div>
            <HomeChild ref={childRef}/>
            <button onClick={btn}>ref Test</button>
        </>
    );
}

export default Home;
```

```jsx
//子组件
import {forwardRef, useImperativeHandle, useState} from "react";

const HomeChild = forwardRef((props, childRef) => {
    const [con, setCon] = useState('content')
    const getCon = () => {
        setCon('321213')
        return con
    }
    useImperativeHandle(childRef, () => ({
            getCon
        }
    ))

    return (
        <>
            <div>
                <p ref={childRef}>this is child</p>
            </div>
        </>
    )
})
export default HomeChild
```

### 注意点

- 子组件需要使用`forwardRef` 包裹整个组件返回整个组件dom（原理不确定）
- 使用 `useImperativeHandle` 包裹父组件可使用的方法与变量
