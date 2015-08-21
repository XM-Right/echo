# Echo.js [![Build Status](https://travis-ci.org/toddmotto/echo.svg)](https://travis-ci.org/toddmotto/echo)
  
  Echo 是一个延迟加载图片独立极小的javaScript库，大小只有2kb，并且使用 HTML5 的data-*属性使其简单，查看实例[demo](http://toddmotto.com/labs/echo)。 Echo只能工作在 IE8+。

Echo is a standalone JavaScript lazy-loading image micro-library. Echo is fast, 2KB, and uses HTML5 data-* attributes for simple. Check out a [demo](http://toddmotto.com/labs/echo). Echo works in IE8+.

```
bower install echojs
```
使用Echo.js很简单，想要立即添加图片到网页中只需要简单的在 img 标签上添加 'data-echo' 属性，或者如果你使用Echo 延迟加载在一个元素的背景图片上 只需要为这个元素的 'data-echo-background' 属性加入图片的URL。

Using Echo.js is simple, to add an image directly into the page simply add a `data-echo` attribute to the img tag. Alternatively if you want to use Echo to lazy load background images simply add a `data-echo-background' attribute to the element with the image URL.

```html
<body>

  <img src="img/blank.gif" alt="Photo" data-echo="img/photo.jpg">

  <script src="dist/echo.js"></script>
  <script>
  echo.init({
    offset: 100,
    throttle: 250,
    unload: false,
    callback: function (element, op) {
      console.log(element, 'has been', op + 'ed')
    }
  });

  // echo.render(); is also available for non-scroll callbacks
  </script>
</body>
```

## .init() (options)

这个 `init()` API 没有选项。
The `init()` API takes a few options

#### offset
Type: `Number|String` Default: `0`

这个 `offset` 选项运允许你指定在视窗的上下左右多远的距离使用Echo开始加载你的图片，如果你指定 `0`，Echo 将会很快加载你的图片显示到这个视窗，如果你想加载这个视窗上下1000px内的图片，指定 `1000`。

The `offset` option allows you to specify how far below, above, to the left, and to the right of the viewport you want Echo to _begin_ loading your images. If you specify `0`, Echo will load your image as soon as it is visible in the viewport, if you want to load _1000px_ below or above the viewport, use `1000`.

#### offsetVertical
Type: `Number|String` Default: `offset`'s value

这个 `offsetVertical` 选项允许你指定在视窗的上面和下面多远的距离使用 Echo 加载你的图片。

The `offsetVertical` option allows you to specify how far above and below the viewport you want Echo to _begin_ loading your images.

#### offsetHorizontal
Type: `Number|String` Default: `offset`'s value

这个 `offsetHorizontal` 选项允许你指定在窗口的左边和右边多远的距离使用 Echo 加载你的图片。

The `offsetHorizontal` option allows you to specify how far to the left and right of the viewport you want Echo to _begin_ loading your images.

#### offsetTop
Type: `Number|String` Default: `offsetVertical`'s value

这个 `offsetTop` 选项允许你指定在这个视窗的上面多远的距离使用 Echo 加载你的图片。

The `offsetTop` option allows you to specify how far above the viewport you want Echo to _begin_ loading your images.

#### offsetBottom
Type: `Number|String` Default: `offsetVertical`'s value

这个 `offsetBottom` 选项允许你在这个视窗下面多远的距离使用 Echo 加载你的图片。

The `offsetBottom` option allows you to specify how far below the viewport you want Echo to _begin_ loading your images.

#### offsetLeft
Type: `Number|String` Default: `offsetVertical`'s value

这个 `offsetLeft` 选项允许你在这个视窗左边多远的距离使用 Echo 加载你的图片。

The `offsetLeft` option allows you to specify how far to left of the viewport you want Echo to _begin_ loading your images.

#### offsetRight
Type: `Number|String` Default: `offsetVertical`'s value

这个 `offsetLeft` 选项允许你在这个视窗右边多远的距离使用 Echo 加载你的图片。

The `offsetRight` option allows you to specify how far to the right of the viewport you want Echo to _begin_ loading your images.

#### throttle
Type: `Number|String` Default: `250`

这个 `throttle` 是由某个内部函数用来阻止 `window.onScroll` 连续触发的问题，使用这个 `throttle` 在用户滚动页面的时候将会设置一个超时时间直到用户停止滚动为止。这个默认值是 `250` 毫秒。

The throttle is managed by an internal function that prevents performance issues from continuous firing of `window.onscroll` events. Using a throttle will set a small timeout when the user scrolls and will keep throttling until the user stops. The default is `250` milliseconds.

#### debounce
Type: `Boolean` Default: `true`

默认情况下， `throttle` 实际上是一个抵消功能，所以检验功能只有是在用户停止滚动的时候才触发的。要使用传统的 `throttle` 检验每一毫秒， 设置 `debounce` 为 `false`。

By default the throttling function is actually a [debounce](http://underscorejs.org/#debounce) function so that the checking function is only triggered after a user stops scrolling. To use traditional throttling where it will only check the images every `throttle` milliseconds, set `debounce` to `false`.

#### unload
Type: `Boolean` Default: `false`

This option will tell echo to unload loaded images once they have scrolled beyond the viewport (including the offset area).

#### callback
Type: `Function`

The callback will be passed the element that has been updated and what the update operation was (ie `load` or `unload`). This can be useful if you want to add a class like `loaded` to the element. Or do some logging.

```js
echo.init({
  callback: function(element, op) {
    if(op === 'load') {
      element.classList.add('loaded');
    } else {
      element.classList.remove('loaded');
    }
  }
});
```

## .render()

Echo's callback `render()` can be used to make Echo poll your images when you're not scrolling, for instance if you've got a filter layout that swaps images but does not scroll, you need to call the internal functions without scrolling. Use `render()` for this:

```js
echo.render();
```

Using `render()` is also throttled, which means you can bind it to an `onresize` event and it will be optimised for performance in the same way `onscroll` is.

## Manual installation
Drop your files into your required folders, make sure you're using the file(s) from the `dist` folder, which is the compiled production-ready code. Ensure you place the script before the closing `</body>` tag so the DOM tree is populated when the script runs.

## Configuring Echo
Add the image that needs to load when it's visible inside the viewport in a `data-echo` attribute:

```html
<img src="img/blank.gif" alt="Photo" data-echo="img/photo.jpg">
```

## Contributing
In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using Gulp.

## License
MIT license
