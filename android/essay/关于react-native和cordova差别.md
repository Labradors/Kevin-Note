## 关于`react-native`和`cordova`差别

- 版本升级后的向下兼容性。相同。
- 风险程度。`Native`比`cordova`高。
- 插件完善度。`Native`的`Android`端不是很完善，有些地方需要自定义。而`cordova`这方面没有问题。
- 对于开发者的要求程度。`cordova`是完全基于`html`，`css`，`js`。而`react-native`是基于`native`的。相比之下`react-native`门槛较高些。因为`cordova`是写一次代码，适用所有平台。而`Native` 是分别写不同的平台，只是语言的语法体系一样。
- 加载速率，`react-native`更好。

## `WebView`问题

因为`Android WebView`和`IOS`的`UIWebView`内存泄露的问题。所以在选择内核的时候，使用原生的`WebView`内存泄露很明显。并且不易解决。IOS8+`之前，同样有大量内存泄露。并且`IOS api`的改进比较激进。如果要考虑`Android4.4`以下的设备。因为前后的运行内核不一。性能不一。有如下几个选择：

- 使用`Crosswalk`开源`web`引擎。

  ### 优势

  1. 更丰富的HTML5特性支持。包括`WebGL`，`WebAudio`，`WebRTC`，`Gamepad`，`WebSocket`等等。
  2. 使用Crosswalk可以保持平台的一致性。

  ### 劣势

  1. 打包后的`app`体积增加*20M-30M*。
  2. Crosswalk lite针对上面第一条，`CrossWalk`提出了`Shared Mode`和`Crosswalk lite`解决方案。体积可以减少到只增加*10M*左右。

- 使用腾讯TBS浏览服务

  ### 优势

  1. 速度快：相比系统webView的网页加载速度有近30%的提升。
  2. 大小只有 *253K。*
  3. 省流量：云端优化技术使流量节省20%。
  4. 更安全：24小时安全问题解决机制
  5. 更稳定：经过亿级用户的使用考验，CRASH率0.15%
  6.  集成强大的视频播放器，支持各种视频格式直接打开
  7. 适屏排版、字体设置等浏览增强功能的提供
  8. `Html5`更完整支持。
  9. 无系统内核的碎片化问题，更少的兼容性问题

  ###  劣势

  1. `X5SDK`是通过调用微信/手机QQ/空间的X5内核。如果手机没有安装腾讯相关软件。这个就不能使用。


- 使用[SUI Mobile (MSUI)](https://github.com/sdc-alibaba/SUI-Mobile)来构建更漂亮的移动端应用。
- [7个优秀的国内外移动端web框架](http://codecloud.net/9427.html)。


## 推荐阅读

[ionic ，react-native , native 移动端开发对比](http://www.yliyun.com/blog/2015/11/16/ionic-react-native-native-yi-dong-duan-kai-fa-dui-bi/)