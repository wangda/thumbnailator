# thumbnailator
自动导入的

此文来自:http://www.oschina.net/question/76860_25758 

## 1.简单介绍

借用红薯对Thumbnailator 的描述：Thumbnailator是一个用来生成图像缩略图的 Java类库，通过很简单的代码即可生成图片缩略图，也可直接对一整个目录的图片生成缩略图。

有了这玩意，就不用在费心思使用Image I/O API,Java 2D API等等来生成缩略图了。

直接上代码，先来看一个最简单的例子：



的确是爽歪歪的说，一行代码就把大鸟变小鸟。

那我要是有一个文件夹都需要生成缩略图，那还是很麻烦，有没有对文件夹下所有图片生成缩略图呢？答案是肯定的：

    Thumbnails.of(new File("path/to/directory")
    .listFiles())         
    .size(640, 480)         
    .outputFormat("jpg")         
    .toFiles(Rename.PREFIX_DOT_THUMBNAIL);

       这个代码想不用我解释就能看懂什么意思了吧？我个人很喜欢这种API的方式，简洁，易懂，明了。

## 2．特点

### 2.1.可以根据现有的图片生成高质量的缩略图

    下面是一个对比：







Thumbnailator生成的缩略图

Graphics.drawImage生成的缩略图

### 2.2.可以在缩略图中嵌入水印，并且可以设置水印的透明度：

### 2.3.支持生成经过旋转后的缩略图：

代码：
    for (int i : new int[] {0, 90, 180, 270, 45}) {
        Thumbnails.of(new File("coobird.png"))
            .size(100, 100)
            .rotate(i)
            .toFile(new File("image-rotated-" + i + ".png"));
    }
### 2.4.可以生成多种质量模式的缩略图

### 2.5.如果需要的话，在生成缩略图的时候可以保持和源图像一样的的宽高比

## 3.更多实战例子

### 3.1.最简单的例子

    Thumbnails.of(new File("original.jpg"))
        .size(160, 160)
        .toFile(new File("thumbnail.jpg"));</b>

最后一行的toFile()方法还接受一个String类型的参数，如下面的代码和上面的作用的一样的：


    Thumbnails.of("original.jpg")
        .size(160, 160)
        .toFile("thumbnail.jpg");

### 3.2.生成一个带有旋转和水印的缩略图：

    Thumbnails.of(new File("original.jpg"))
        .size(160, 160)
        .rotate(90)
        .watermark(Positions.BOTTOM_RIGHT, ImageIO.read(new File("watermark.png")), 0.5f)
        .outputQuality(0.8f)
        .toFile(new File("image-with-watermark.jpg"));</b>

这段代码是从original.jpg这张图片生成最大尺寸160*160，顺时针旋转90°，水印放在右下角，50%的透明度，80%的质量压缩的缩略图。

### 3.3.把生成的图片输出到输出流（OutPutStream）中

    OutputStream os = ...;
                 
    Thumbnails.of("large-picture.jpg")
        .size(200, 200)
        .outputFormat("png")
        .toOutputStream(os);</b>

### 3.4.按一定的比例生成缩略图

    BufferedImage originalImage = ImageIO.read(new File("original.png"));
 
    BufferedImage thumbnail = Thumbnails.of(originalImage)
        .scale(0.25f)
        .asBufferedImage();</b>

生成缩略图的大小是原来的25%

 

整理翻译自：

http://code.google.com/p/thumbnailator/

http://code.google.com/p/thumbnailator/wiki/Examples

 

Thumbnailator的下载地址：

http://code.google.com/p/thumbnailator/downloads/list

 

Java Doc

http://thumbnailator.googlecode.com/hg/javadoc/index.html
