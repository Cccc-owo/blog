---
title: EU4的Bitmap font字体贴图制作
tags:
  - EU4
  - Bitmap-font
categories:
  - 技术
comments: true
sitemap: true
abbrlink: 8b51873a
date: 2022-08-23 15:09:18
updated: 2024-09-26 15:57:00
---

> Bitmap font 是 P 社的 Clausewitz 引擎所使用的一种字体格式（当然现在也能用 TTF），这种字体的特点是所有文字都是栅格化的，不同于 ttf 等矢量格式的字体。所有字符绘制在一张或多张像素图片（通常为 dds 或 tga）上，再通过一个定义文件（fnt）来描述每个字符在图片中的位置，引擎就可以以此配置来读取并将字符显示到界面中。

<!-- more -->

---

## 前言

在EU4的游玩过程中，你是否会感觉汉化自带的地图字体不够美观？

本文就来分享我制作EU4地图字体的经验。可以[ParaTranz](https://paratranz.cn/blog?category=document)所给出的文档作为补充，有冲突时请以本文为准

## 字体选择？

> ParaTranz站长的小贴士：
>
> 建议选择 GBK 字符集的字体，可以从[方正字体](http://www.foundertype.com/)的官网上选择一款你喜欢的字体

## 生成地图字体文件

下载[Bitmap font generator](http://www.angelcode.com/products/bmfont/bmfont64_1.14b_beta.zip)（后文简称bmfont）

首先打开bmfont，先看到左上角菜单栏的两个选项，我们将依次解释。

点击Options，会显示出如下选项：

|Options|翻译|
|-----|-----|
|Font settings|字体设置|
|Export option|导出选项|
|Visualize|可视化|
|Save bitmap font as...|保存字体位图为……|
|===|===|
|Abort generation|停止生成|
|===|===|
|Load configuration|加载配置|
|Save configuration as...|保存配置为……|
|===|===|
|About...|关于|
|Exit|退出|

### 选项解释

#### Font settings 字体设置

首先看到```Font settings```：

在```Font graphics```中，

```Font```栏用于指定用于生成贴图的字体，而```Add font file```指定用于生成贴图的字体ttf文件，一般而言，我们使用上面这个Font栏指定字体即可，只要是系统安装的字体都在其中

接着看到```Charset```，即字符集，我们选择```Unicode```，不用管下面的OEM字符集

再到```Size(px)```，即字符大小。对于地图字体，由于常常需要显示得比较大，因此不建议将这个值设置得太小，在(80,100)之间是我个人比较推荐的，我的系列字体即88px大小。右方有一个```Match char height```选项，不必勾选

下面有一些关于```Autofit pages```的选项，我们不需要这个，因为EU4的字符贴图文件似乎只能有一个，因此我们应该尽量使全部字符在一页内生成，所以保留```Autofit pages:0```

对于```Height %```，我们也是采用默认的100即可，右方有```Bold```（机器加粗）和```Italic```（斜体），均不需勾选

下面还有两个选项，分别是```Output invalid char glyph```和```Do not include kerning pairs```，也不需勾选。

再看到下面的```Rasterization```，我们勾选```Render from TrueType outline```，```TrueType hinting```，```Font smoothing```，三项，至于```Super sampling```（超级采样），属于可选项，似乎能提高清晰度，但是效果好像不太明显，而且会减慢生成速度，这个就自己看着选择吧，我个人一般不开。

还有最下方的```Effects```，对于```Outline thickness```（描边厚度），我这里填0，可以方便后续使用专业工具进行进一步处理。

完成此页面设置，点击```OK```保存。

#### Export Options 导出选项

到了```Export Options```，我们会发现这个设置就比较复杂了。还是先看到```-Layout-```一栏：

```Padding```，即字符之间的间距，建议为0到2之间，如果不需要后期处理，那么采用0即可；而如果希望进行一些后期处理（如阴影等），建议调为1~2。

```Spacing```，游戏中体现出来的文字间隔，由于P社有自己的显示方法，这里我们都设为0即可。

```Equalize the cell heights```选项，应该是使字符高度相同的意思，没必要勾选。

```Force offsets to zero```选项，强制设定偏移量为零，不知道有什么用，我一般不勾。

再来到下面的```-Texture-```栏：

```Width```，```Height```即输出文件的宽和高，这两个值应为4的整数倍，至于大小则应该视你生成的字符贴图而定，一般来说，我们追求的是让字符在一张贴图中显示的同时尽可能使文件更小一点，这可能就需要几次尝试了。

```Bit depth```，即位深，可以理解为值越大，图像越清晰，这里我们选择32。

选择```Bit depth```为32后，可以看到```Pack chars in multiple channels```这个选项，不要勾选这个，接着再往下看。

下面就是比较重要的设置——ARGB通道的输出。这里我们只浅浅地介绍一下（因为我也不太懂）。```Chnl```表示通道，```Value```表示输出类型，```Invert```可以理解为反色。我们直接看到下面的```Presets```，有两种建议的预设，```White text with alpha```是带透明的白色字体，而```Black text with alpha```则是带透明的黑色字体，一般我们只需要这两种设定就可以了。

最后，```-File format-```：

```Font descriptor```选择```Text```

```Textures```选择```dds - DirectDraw Surface```

```Compression```建议选择```None```，**这是为了后期处理不丢失图像质量**，但是如果你**并不打算进行任何后期处理**，那么就请选择```DXT3```

设置完毕后，点击OK保存设置。

#### Visualize 可视化

这个其实没什么必要，点击之后会生成图像预览，不如直接导出图像更好。

#### 剩余几项……

不必多说，看懂中文大概也懂个什么意思了。

### Edit 编辑

这里是关于选择字符的设定。

|Edit|翻译|
|-----|-----|
|(Un)Select all chars|（取消）选择全部字符（当前页内的）|
|Select marked subset(s)|选择标记的子集|
|Unselect marked subset(s)|取消选择标记的子集|
|Clear all chars in font|清除字体中所有字符（已选中的）|
|===|===|
|Select chars from file|从文件中选择字符|
|===|===|
|Find next failed character|查找下一个失败的字符|
|Clear failed characters|清除失败的字符|
|===|===|
|Open Image Manager|打开图像管理器（用处似乎不大？）|

比较重要的选项有：

```Select chars from file```可以从一个文本文件（txt）中选中其中的字符，这样就不用手动地一个个选了。注意这个文件的编码应该是 UTF8 with BOM 格式的，否则会识别错乱。可以多次叠加从文件中选中的字符。

```Clear all chars in font```清除字体中所有已选中的字符，用于重置已选择的字符。

其他不多解释。

### 生成字体贴图

仔细阅读完以上解释后，确保设置无误，并选中所有你所希望包括的字符。现在请转到菜单栏中```Options```的```Save bitmap font as..```，我们可以导出字体贴图了。生成文件名请写为```zh-hans-map```，以替换汉化文件中的地图字体贴图。

**注意！**，要控制dds文件的大小，**过大（大约58MB左右）则可能会导致无法显示文字**。如果你选择的```Export settings```中的```Compression```选项不是```None```的话，那么文件大小超过100MB是正常的，不要慌，这是因为压缩这个步骤被我放在了后期处理的最后，我们继续处理。

### 字体图像的后期处理

带\*号的标题意思是如果你在```Export settings```中的```Compression```选项不是```None```的话，不太建议进行这些操作，因为会损失图像质量。

这里我们使用[GIMP](https://www.gimp.org/downloads/)作为示例。

#### \*阴影

使用GIMP打开生成的dds图像，转到上方菜单栏中的 滤镜->光照和阴影->投影。

参数设置如下：

X,Y为0

Blur radius为1左右

Grow shape随意选择

Grow radius为1左右

Color为3f3f3f（灰色之中选择你认为合适的）

Opacity为0.400左右

注意以上设置仅供参考，可以自己继续研究如何效果更好。

#### \*不透明度

100%不透明度可能视觉效果上过于突出了，有时候稍微带一点透明的感觉会更好，我建议设置不透明度为75~85%

#### GIMP导出

按下快捷键 Ctrl+Shift+E，导出文件，注意格式始终应该是.dds！

#### \*压缩贴图

推荐使用[NVIDIA Texture Tools Exporter](https://developer.nvidia.cn/texture-tools-exporter)，导出带Mipmap的压缩dds效果更好。

我们打开NVIDIA Texture Tools Exporter，导入所需处理的dds文件，保持其他选项不变，只改动以下提到的选项：

```Format```改为```BC2```，```Mipmap Options```中，```Filter Type```可以任选，我个人不太清楚其中差异。

设置完成后，点击右下角的```Save As...```，文件名存为```zh-hans-map.dds```，到此，大功即将告成。

#### 修改文件名

一般来说，bmfont生成的字体贴图dds文件名是\*\*\*\*\*\*\*_0.dds（\*\*\*\*\*\*\*表示你在bmfont中导出字体贴图所填的文件名），我们将其修改为\*\*\*\*\*\*\*.dds，即去掉```_0```，以与汉化文件的文件名保持一致。

### 处理字体描述符文件

找到\*\*\*\*\*\*\*.fnt，用至少功能完备一点的文本编辑器（如Notepad4，VS Code等，注意**不要使用记事本！**）打开，

首先，删除第3、第4行（page 及 chars 定义），以及第2行的```packed=0 alphaChnl=0 redChnl=3 greenChnl=3 blueChnl=3```这一段，还有删除第1行的```unicode=1```。删除之后每行结尾的```chnl=15```。

保存即可。

## 使用

直接替换汉化mod中的对应文件，相对位置在 gfx/fonts/zh-hans-map.\*\*\* 中，进入游戏即可生效。

如果要实现sub mod的效果，可以参考我的字体mod的描述符文件。

感谢你看到这里，欢迎在评论区中交流！

## 附一些字符文件

供各位撷取，其中*古籍印刷标准字*应该视情况导入（因为包含字符太多，可能导致文件体积过大而无法加载）。

[ParaTranz所提供的字符文件](https://static.paratranz.cn/media/chars.txt)

[gb_t_12345标准](https://blog.iscccc.eu.org/chars/1gb_t_12345.txt)

[gbk标准](https://blog.iscccc.eu.org/chars/1gbk.txt)

[古籍印刷标准字](https://blog.iscccc.eu.org/chars/1gujiyinshua.txt)

[汉仪字库使用字](https://blog.iscccc.eu.org/chars/1hanyi_jianfan.txt)

[国家通用规范字](https://blog.iscccc.eu.org/chars/1tongyong_guifan.txt)

<!-- Q.E.D. -->