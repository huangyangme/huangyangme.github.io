# 为什么要 WebP？

图片是流量消耗大头。不仅存储流量费用使用越来越多，客户端打卡图片速度慢。

WebP（发音 weppy，[项目主页](~https://developers.google.com/speed/webp/~)），是一种支持有损压缩和无损压缩的图片文件格式，派生自图像编码格式 VP8。根据 Google 的测试，无损压缩后的 WebP 比 PNG 文件少了 45％ 的文件大小，即使这些 PNG 文件经过其他压缩工具压缩之后，WebP 还是可以减少 28％ 的文件大小。

## 先来个实际的对比：

![](/~http:/of0wpiquw.bkt.clouddn.com/webpduizhao.png~)

- 原图 **83.9KB**，[下载](~http://of0wpiquw.bkt.clouddn.com/demo.jpg~)
- jpg 压缩输出 **29.9KB**，[下载](~http://of0wpiquw.bkt.clouddn.com/demo.jpg?imageView2/0/format/jpg~)
- webp 输出 **16.4KB**，[下载](~http://of0wpiquw.bkt.clouddn.com/demo.jpg?imageView2/0/format/webp~)

可以看出 WebP 格式图片比原图小5倍多，即使是压缩后的 jpg 格式，也比 webp 格式大差不多1倍。

使用时只要把服务器api给的原图链接（如：/demo.jpg）加上 `?imageView2/0/format/webp` 后缀（如：/demo.jpg?imageView2/0/format/webp）就会加载图片的 WebP 格式版本。

## WebP 格式图片优点

- 更小的文件尺寸
- 更高的质量——与其他相同大小不同格式的压缩图像比较
- 支持半透明

## 使用 WebP 格式图片的好处

- 节省CDN流量：给自己省钱
- 节省用户下载图片流量：给用户省钱，图片加载速度更快
- 可操作性：又拍云和七牛都提供同一张图片 WebP 版本输出配置，只需修改图片后缀，无需额外开发代码
- 兼容性：Chrome 内核浏览器原生支持，安卓 4.0+ 原生支持，iOS 需要 WebP 解析库

简单来说，**WebP 文件小，传输快，提升用户体验。WebP 能够在保障图片质量的前提下，让图片大小平均减少50%左右 ，大大提升传输速度。**

## 总结

行者 Android 端如果不再支持 4.0-，可以使用。

Web 端可以在某个模块（如行者帮）在做好兼容性处理后使用 WebP 降级方案。

iOS 端可以使用解析库，或再等等。

> 
- Android 4.0 以下 WebP 解析库（[链接](~https://github.com/alexey-pelykh/webp-android-backport~)）
- WebP 解析库（[链接](~https://github.com/carsonmcdonald/WebP-iOS-example~)）
- 网页浏览器采用降级处理，支持的输出 WebP格式，不支持的输出 JPEG 或者 PNG 格式。

---

#### 扩展数据：

YouTube 的视频略缩图采用 WebP 格式后，网页加载速度提升了 10%；谷歌的 Chrome 网上应用商店采用 WebP 格式图片后，每天可以节省几 TB 的带宽，页面平均加载时间大约减少 1/3；Google+ 移动应用采用 WebP 图片格式后，每天节省了 50TB 数据存储空间。（来自科技博客 Gig‍‍‍aOM ）

手淘 Native 已经全部支持 WebP。

手淘 iOS 的 webview 是有 Native 提供的组件实现，组件负责去下载 WebP 格式图片，然后转码成 JPG 提供给webview。

腾讯已在一些海量级业务中使用 WebP 格式，如：表情商城、QQ空间相册、QQ空间装扮。[智图](~https://zhitu.isux.us~)项目也来自腾讯ISUX前端团队。

#### 扩展阅读：

- [智图—源于QQ空间图片WebP化的思考](~https://isux.tencent.com/zhitu.html~)
- [探究WebP一些事儿](~https://aotu.io/notes/2016/06/23/explore-something-of-webp/~)
- [WebP 探寻之路](~https://isux.tencent.com/introduction-of-webp.html~)

