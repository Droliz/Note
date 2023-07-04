# gsap动画库

第三方提供的用于提高动画性能（不需要自己计算）

## 下载

```sh
npm install gsap -S
```

## 使用

只是用于修改，还是需要使用`requestAnimationFrame`来实现动画

```js
// 使用 gsap 库的动画函数
gsap.to(cube.position, { x: 5, duration: 5});
// 可以同css动画一样设置
  
// // 默认的参数 time 表示时间
const animate = () => {
  
    // 浏览器自带的动画函数，每次刷新都会调用（每一帧）
    requestAnimationFrame(animate);
    // 更新控制器
    controls.update();
    // 重新渲染
    renderer.render(scene, camera);
};
  
animate();
```

## 回调函数

在动画开始、结束等时候自动调用回调函数

```js
const animation = gsap.to(cube.position, {
    x: 5,
    duration: 5,
    repeat: 2,   // -1 代表无限循环
    onComplete: () => {
        console.log('动画完成');
    },
    onRepeat: () => {
        console.log('动画重复');
    },
    onStart: () => {
        console.log('动画反向完成');
    }
});

// 接收的animation也可以使用 pause() 等方法实现暂停、运动
```

