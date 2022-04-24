# JavaScript and CSS animation showing flowers following the mouse pointer.

This effect is inspired by some websites where when you are moving the mouse around, some bubbles, hearts, or ripples will go around the mouse pointer with appealing animations.

This is a simple example of how to make such an effect using HTML, JavaScript, and CSS.

The idea is we listen to the `mousemove` events, and when these events fire we create these divs that have background images of flowers, hearts, and bubbles. And we set the divs' position to the pointer's. Then we use `setTimeout` to remove them after seconds:

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

To achieve a dynamic and random visual effect for those flowers, we use `Math.random()` which is a very useful function when creating such effects to set a random size and a random rotation degree for each flower:

```js
let size = Math.random() * 80;
flower.style.width = 20 + size + "px";
flower.style.height = 20 + size + "px";

let rotation = Math.random() * 360;
flower.style.transform = `rotate(${rotation}deg)`;
```

That's all for the JavaScript part. We use JavaScript to create DOM elements and set the initial state for them.

The animation part is handled by CSS. We have 3 animations here:

1. Flowers fade in after creation
2. Flowers fade out before deletion
3. Flowers scatter around

Fade in/out we use one `keyframes` to handle:

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

And for scattering, we use the `move` keyframes, to make flowers move away and rotate at the same time.

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

Both keyframes share the same animation duration, timing function, and iteration settings :

```css
div {
  animation: fadeInOutc 2s linear infinite;
}

div:before {
  animation: move 2s linear infinite;
}
```

Another important detail is that when flowers are stacked, it's better to add a shadow for the front ones. We use [drop-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function/drop-shadow) to make it happen.

```css
filter: drop-shadow(0 0 15px rgba(0, 0, 0, 0.5));
```
