# qinPlayer

## 说明

一款基于 react 的 html5 播放器, 支持弹幕字幕以及可拓展的主题

## 预览图

![image.png](https://i.loli.net/2020/04/16/16otDOXuEJ5ypc3.png)
![image.png](https://i.loli.net/2020/04/16/dmwfZPcRJUrD319.png)
![image.png](https://i.loli.net/2020/04/16/13bpmto6xlANOv9.png)
![image.png](https://i.loli.net/2020/04/16/ubLEfJrSwUI5ODx.png)
![image.png](https://i.loli.net/2020/04/16/Y3Hli45PXJ1at2h.png)
![image.png](https://i.loli.net/2020/04/16/i1Zm4gphxy2CPOk.png)

## 开发方向

web 端的功能比较完善, 移动端经过考虑去除了设置, 网页全屏, 音量, 弹幕字幕也仅保留开关;

后续需要测试多浏览器的兼容性;

## 设计理念

```jsx
<PlayerProvider>
  <Theme>
    <Danmu></Danmu>
    <Sub></Sub>
    <Core></Core>
  </Theme>
</PlayerProvider>
```

首先将整个播放器进行分层设计, 各组件之间尽量解耦, 方便日后随时更换组件;

- `<PlayerProvider />` 是数据收集与分发的中心, 主要是将数据与方法分发至不同的组件, 然后各组件根据数据调整状态, 或者执行方法改变核心的状态;
- `<Core />` 是对 HTML5 的 video 更进一步的封装, 主要是将状态回传至 provider , 并监听状态的改变去执行暂停快进等, 以后可能会加个拓展层去支持更多的格式;
- `<Theme />` 是目前的默认主题, 主要是参考了 B 站的设计, 但是实际上是完全解耦的, 可以根据移动端的来源自动切换另一套主题, 同时也可以实现多套主题相互切换;
- `<Danmu />` 是弹幕的处理层, 他覆盖在 video 上面, 但是在点击层的下面, 所以暂时无法点击到, 它可以获取弹幕列表, 并渲染弹幕, 而发送弹幕这个则以 children 的形式传入进去, 外部自行处理逻辑,
- `<Sub />` 是字幕层, 它主要是获取字幕然后解析 WEBVTT 的格式并动态渲染到底部, 由于没有采用原生的字幕显示, 所以在表现上也更加丰富生动, 后续如果想要解析其他格式的话, 也只需要对这一层加强即可;

## 问题

从理论上来说, 设计上还是比较合理的, 但是 video 频繁的数据回调可能会导致整个的播放器渲染卡顿等, 目前也是在减少其他层的数据依赖,需要进行压测以及一些交互优化

## 兼容性

目前在 chrome, safari, firefox, edge 使用均没有大问题, 至于 IE 已经被彻底忽略
