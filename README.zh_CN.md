# JavaScript and CSS animation showing flowers following the mouse pointer.

这个效果的灵感来自某些网站，当鼠标移动时，鼠标箭头周围出现气泡、红心或水波的扩散图案，这种效果对网页的访问者有很强的视觉吸引力

我们用原生的 HTML，JavaScript 和 CSS 就能实现这种效果

## 思路如下：

我们监听`mousemove`事件，当事件触发时，创建一些 `div`，花朵，红心和气泡作为`div`的背景图片。并将他们的位置设置为鼠标指针的位置，然后使用`settimeout`在几秒钟之后将其删除后：

```js
document.addEventListener("mousemove", function (e) {
  let body = document.querySelector("body");
  let flower = document.createElement("div");
  let x = e.offsetX;
  let y = e.offsetY;
  flower.style.left = x + "px";
  flower.style.top = y + "px";
  body.appendChild(flower);
  setTimeout(function () {
    flower.remove();
  }, 2000);
});
```

为了使这些花儿呈现动态和随机的视觉效果，我们使用 `Math.random()` 为每朵花儿设置了随机尺寸和随机旋转角度（ `Math.random()` 在实现类似的效果时总是非常有用）。

```js
let size = Math.random() * 80;
flower.style.width = 20 + size + "px";
flower.style.height = 20 + size + "px";

let rotation = Math.random() * 360;
flower.style.transform = `rotate(${rotation}deg)`;
```

以上就是 JavaScript 的部分了, 我们主要用 JS 进行元素的创建和初始状态的设定。

动画效果的部分使用 CSS 就能做到。总共要实现 3 个动画效果：

1. 花儿创建之后淡入
2. 花儿被移除之前淡出
3. 花儿的移动扩散和旋转

淡入淡出通过设置几个节点的 `opacity`实现：

```css
@keyframes fadeInOut {
  0%,
  100% {
    opacity: 0;
  }
  20%,
  80% {
    opacity: 1;
  }
}
```

移动扩散和旋转用 `transform` 实现即可：

```css
@keyframes move {
  0% {
    transform: translate(0) rotate(0deg);
  }
  100% {
    transform: translate(300px) rotate(360deg);
  }
}
```

这两个 `keyframes` 设置一样的时常、时间函数都用 `linear`，重复次数都设置为 `infinite`:

```css
div {
  animation: fadeInOutc 2s linear infinite;
}

div:before {
  animation: move 2s linear infinite;
}
```

最后，还有一个小效果值得一提，即当两片花儿重叠的时候，上面的花要在下面的花上投入一个阴影，这样会显得更逼真，这个效果可以用 [drop-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function/drop-shadow) 实现。

```css
filter: drop-shadow(0 0 15px rgba(0, 0, 0, 0.5));
```

希望大家喜欢这个效果！

最后附上：

Demo：https://codesteppe.github.io/pointer-flowers/

源码：https://github.com/CodeSteppe/pointer-flowers
