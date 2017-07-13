
# Understanding the most commonly used image file formats

Anybody who is making figures for data visualization will eventually have to know a few things about how figures are stored on the computer. There are many different image file formats, and each has its own set of benefits and disadvantages. Choosing the right file format and the right workflow can alleviate many figure-preparation headaches. 

My own preference is to use pdf for high-quality publication-ready files and generally whenever possible, png for online documents and other scenarios where bitmap graphics are required, and jpeg as the final resort if the png files are too large. In the following, I explain the key differences between these file formats and their respective benefits and drawbacks.

## Bitmap and vector graphics

The most important difference between the various graphics formats is whether they are bitmap or vector (Table \@ref(tab:file-formats)). Bitmaps or raster graphics store the image as a grid of individual points (called pixels), each with a specified color. By contrast, vector graphics store the geometric arrangement of individual graphical elements in the image. Thus, a vector image contains information such as "there's a black line from the top left corner to the bottom right corner, and a red line from the bottom left corner to the top right corner," and the actual image is recreated on the fly as it is displayed on screen or printed.

Table: (\#tab:file-formats) Commonly used image file formats

Acronym   Name                              Type         Application
--------- --------------------------------- ------------ ------------------------
pdf       Portable Document Format          vector       general purpose
eps       Encapsulated PostScript           vector       general purpose, outdated; use pdf
svg       Scalable Vector Graphics          vector       online use
png       Portable Network Graphics         bitmap       optimized for line drawings
jpeg      Joint Photographic Experts Group  bitmap       optimized for photographic images
tiff      Tagged Image File Format          bitmap       print production, accurate color reproduction
raw       Raw Image File                    bitmap       digital photography, needs post-processing
gif       Graphics Interchange Format       bitmap       outdated, do not use

Vector graphics are also called "resolution-independent."



(ref:iris-zoom) Illustration of the key difference between vector graphics and bitmaps. 

<div class="figure" style="text-align: center">
<img src="figures/iris_zoom.png" alt="(ref:iris-zoom)" width="2430" />
<p class="caption">(\#fig:iris-zoom)(ref:iris-zoom)</p>
</div>


*May want to mention that vector graphics have downsides, in that sometimes the interpretation of the same code differs among programs. Particularly a problem with fonts, and more frequently with svg than pdf.*

## Lossless and lossy compression of bitmap graphics

Most bitmap file formats employ some form of data compression to keep file sizes manageable. There are two fundamental types of compression: lossless and lossy. Lossless compression guarantees that the compressed image is pixel-for-pixel identical to the original image, whereas lossy compression accepts some image degradation in return for smaller file sizes.

To understand which approach is appropriate when, it is helpful to have a basic understanding of how these different compression algorithms work. Let's first consider lossless compression. Imagine an image with a black background, where large areas of the image are solid black and thus many black pixels appear right next to each other. Each black pixel can be represented by three zeroes in a row: 0 0 0, representing zero intensities in the red, green, and blue color channels of the image. The areas of black background in the image correspond to thousands of zeros in the image file. Now assume somewhere in the image are 1000 consecutive black pixels, corresponding to 3000 zeros. Instead of writing out all these zeros, we could store simply the total number of zeros we need, e.g. by writing 3000 0. In this way, we have conveyed the exact same information with only two numbers, the count (here, 3000) and the value (here, 0). Over the years, many clever tricks along these lines have been developed, and modern lossless image formats (such as png) can store bitmap data with impressive efficiency. However, all lossless compression algorithms perform best when images have large areas of uniform color, and therefore Table \@ref(tab:file-formats) lists png as optimized for line drawings.

Photographic images rarely have multiple pixels of identical color and brightness right next to each other. Instead they have gradients and other somewhat regular patterns on many different scales. Therefore, lossless compression of these images often doesn't work very well, and lossy compression has been developed as an alternative. The key idea of lossy compression is that some details in an image are too subtle for the human eye, and those can be discarded without obvious degradation in the image quality. For example, consider a gradient of 1000 pixels, each with a slightly different color value. Chances are the gradient will look nearly the same if it is drawn with only 200 different colors and coloring every five adjacent pixels in the exact same color.

The most widely used lossy image format is jpeg (Table \@ref(tab:file-formats)), and indeed many digital cameras output images as jpeg by default. Jpeg compression works exceptionally well for photographic images, and huge reductions in file-size can often be obtained with very little degradation in image quality. However, jpeg compression fails when images contain sharp edges, such as created by line drawings or by text. In those cases, jpeg compression can result in very noticeable artifacts (Figure \@ref(fig:jpeg-example)). 


(ref:jpeg-example) Illustration of jpeg artifacts. (a) The same image is reproduced multiple times using increasingly severe jpeg compression. The resulting file size is shown in the top-right corner of each image. A reduction in file size by a factor of 10, from 432kB in the original image to 43kB in the compressed image, results in only minor perceptible reduction in image quality. However, a further reduction in file size by a factor of 2, to a mere 25kB, leads to numerous visible artifacts. (b) Zooming in to the most highly compressed image reveals the various compression artifacts. *Image credit: Claus O. Wilke* 

<div class="figure" style="text-align: center">
<img src="figures/jpeg_example_combined.jpg" alt="(ref:jpeg-example)" width="806" />
<p class="caption">(\#fig:jpeg-example)(ref:jpeg-example)</p>
</div>


## Converting between image formats

It is generally possible to convert any image format into any other image format. For example, on a Mac, you can open an image with Preview and then export to a number of different formats. In this process, though, important information can get lost, and information is never regained. For example, after saving a vector graphic into a bitmap format, e.g. a pdf file as a jpeg, the resolution independence that is a key feature of the vector graphic has been lost. Conversely, saving a jpeg image into a pdf file does not magically turn the image into a vector graphic. The image will still be a bitmap image, just stored inside the pdf file. Similarly, converting a jpeg file into a png file does not remove any artifacts that may have been introduced by the jpeg compression algorithm.

It is therefore a good rule of thumb to always store the original image in the format that maintains maximum resolution, accuracy, and flexibility. Thus, for data visualizations, create your figure as pdf and then convert into png or jpg when necessary. Similarly, for images that are only available as bitmaps, such as digital photographs, store them in a format that doesn't use lossy compression, or if that can't be done, compress as little as possible. Also, store the image in as high a resolution as possible, and down-scale when needed.  
