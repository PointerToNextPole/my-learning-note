# Media



#### Ogg格式

Ogg全称是OGGVobis(oggVorbis)是一种<font color=FF0000>音频压缩格式</font>，类似于MP3等的音乐格式。Ogg是完全免费、开放和没有专利限制的。OggVorbis文件的扩展名是".ogg"。<font color=lightSeaGreen>Ogg文件格式可以不断地进行大小和音质的改良，而不影响旧有的编码器或播放器</font>。



//todo

#### HSL格式

HSL（色相，饱和度，明度）



## 图片

##### 图片选择场景与方案

> 👀 该表格摘自 [前端如何选择图片格式？现在有了全新的回答！](https://mp.weixin.qq.com/s/St68rT0ExK0Jo7SEFjRfAQ) 。另外，该方案推出的背景是：25/6/24 W3C 正式发布了 PNG 第三版规范：不仅修复了既有问题，还引入了 HDR、高质量动画 ( APNG )、Exif 元数据等现代图像需求所需的新功能；具体见 [[#PNG 第三版规范]]

| **场景**                  | **推荐格式**      | **理由**                                       |
| :------------------------ | :---------------- | :--------------------------------------------- |
| 图标、透明图层            | PNG / SVG         | 支持透明，渲染清晰，SVG 具备缩放不失真优势     |
| 静态照片或插图展示        | JPEG / WebP / PNG | PNG 支持 HDR，适合需要更高色彩还原的图像       |
| 动态图标 / 动图展示       | APNG / WebP       | 画质更优于 GIF，现代浏览器兼容性良好           |
| 高质量摄影图片            | PNG（HDR + Exif） | 无损质量，完整保留拍摄元数据                   |
| 极致压缩、弱网优化场景    | WebP / AVIF       | 体积更小，加载更快，适合移动端或图片密集型页面 |
| 矢量图标、插画            | SVG               | 可缩放、可压缩，适合 UI 元素                   |
| 数据图、UI 截图等精细图像 | PNG               | 保留边缘清晰度，无损压缩显示                   |

##### PNG 第三版规范

> 👀 官方介绍：[Portable Network Graphics (PNG) Specification (Third Edition)](https://www.w3.org/TR/png-3/)

###### 新功能

- **HDR（高动态范围）支持：**随着显示设备不断进化，HDR 内容日益普及。PNG 第三版通过引入轻量级的元数据块实现对 HDR 图像的原生支持。
  - **优势**：相较 JPEG，PNG 能保留更丰富的亮部细节和更真实的色彩层次。
  - **文件大小影响**：极小，<font color=red>仅增加 4～20 字节</font>，<font color=lightSeaGreen>不影响图像主体压缩效率</font>。
  - **应用场景**：产品渲染图、宣传插图、高质量摄影作品等对视觉还原要求高的图像。
- **官方动画支持 ( APNG )：**虽然 Mozilla 早在 2008 年提出 APNG 格式，但<font color=lightSeaGreen>**直到第三版规范，它才被正式纳入标准体系**</font>。APNG 继承了 PNG 的无损压缩、真彩色和透明通道优势，<font color=lightSeaGreen>同时提供了帧动画能力</font>。
  - **优势**：<font color=lightSeaGreen>画质远超 GIF，支持 24 位色彩和 alpha 透明，适合高保真 UI 动效</font>。
  - **文件大小影响**：取决于帧数和压缩优化，<font color=red>通常略大于 GIF，但视觉质量更高</font>；支持帧差优化进一步减小体积。
  - **应用场景**：动态图标、帧动画插画、现代表情包、按钮动效等。
- **Exif 元数据嵌入：**PNG 现已支持嵌入摄影类元数据 ( Exif )，包括拍摄时间、GPS 坐标、设备信息等，使用 `eXIf` 块记录。这项能力使得 PNG 与 JPEG 在图像存档和智能处理场景中实现了功能对齐。
  - **优势**：无需退回 JPEG，即可在无损图像中记录拍摄信息，更适用于专业工作流。
  - **文件大小影响**：可控，通常增加几十至几百字节。
  - **应用场景**：摄影图库、上传归档、AI 图像识别、照片管理等。

###### 对体积的影响

| **功能**        | **是否增加体积** | **增加幅度说明**                                             |
| :-------------- | :--------------- | :----------------------------------------------------------- |
| HDR 支持        | 否 / 极小        | 仅增加几个字节的色彩描述信息，不影响图像数据压缩率。         |
| 官方动画(APNG)  | 是（可控）       | 动画图像体积随帧数增长，通常比 GIF 略大，但画质更高；帧差优化可减小体积。 |
| Exif 元数据嵌入 | 是（可控）       | 元数据一般为几十至几百字节，对整体文件大小影响轻微。         |

也就是说：

- 对于**普通静态图**，PNG 文件体积基本不变；
- 对于**需要 HDR 或动画的场景**，可以自由选择是否启用；
- <font color=red>新功能是增量兼容的</font>，<font color=lightSeaGreen>老项目不改动也能继续使用</font>。

###### 生态支持情况

新标准已被众多平台和软件广泛采纳，包括：

- **浏览器**：<font color=red>Chrome、Safari、Firefox 等均已支持新版特性</font>
- **操作系统**：Apple 的 iOS 和 macOS 系统原生支持
- **图像处理软件**：如 Adobe Photoshop、DaVinci Resolve 等已实现兼容

摘自：[前端如何选择图片格式？现在有了全新的回答！](https://mp.weixin.qq.com/s/St68rT0ExK0Jo7SEFjRfAQ)