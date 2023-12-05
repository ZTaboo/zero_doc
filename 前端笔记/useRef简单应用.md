```jsx
//父组件
import React, { useRef } from 'react';
import Ref01 from './02_ref_1';

export default function RefLearn() {
  const test = useRef();
  const btn = () => {
    console.log(test.current.zero);
    console.log(test.current.testBtn());
  };
  return (
    <div>
      <Ref01 ref={test}></Ref01>
      <button onClick={btn}>button</button>
    </div>
  );
}
```

```jsx
//子组件
import React, { useState, useImperativeHandle, forwardRef } from 'react';

export default forwardRef(function Ref01(props, ref) {
  const [con, setCon] = useState('123231');
  const btn = () => {
    setCon('hello ref');
  };
  const testBtn = () => {
    console.log('is ok!');
    return '123'
  };
  useImperativeHandle(ref, () => ({
    zero: con,
    testBtn,
  }));
  return (
    <div>
      <div>con:{con}</div>
      <button onClick={btn}>zhanwei</button>
    </div>
  );
});
```
