```css
.menu-font::after{
        font-weight: bold;
        content:'';
        display:block;
        /*开始时候下划线的宽度为0*/
        width:0;
        height:3px;
        left:0;
        bottom:-10px;
        background:#fff;
        /*这里我们设定所有改变都有动画效果，可以自己指定样式才有动画效果*/
        transition:all 0.2s ease-in-out;
    }

.menu-font:hover::after{
        width:100%;
}
```
