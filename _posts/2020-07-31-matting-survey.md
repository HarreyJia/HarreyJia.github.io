---
layout: article
title: Image Matting Techniques Survey
tags: ImageMatting survey
mathjax: true
key: 2020-07-31-matting-survey
---

最近老师让我做一个关于Image Matting方法的Survey，然后我顺便在这里做一个小小的总结（好记性不如烂笔头）。

### 提出的目的（解决的问题）

其实很简单，可以一句话总结：为了满足人类日益增长的对美好生活的向往，修图、短视频、电影等行业大力蓬勃发展。人们在习惯于1080p、蓝光等之后，开始更加关注图像或视频中的细节部分，如：人的发丝、树叶的纹理等。

普通的图像分割（Image Segmentation）更关注于语义的分割，即分出来你、我、他、她即可。但是Matting要做的工作是把一个Object的细节提取得尽量细致且真实，这是为什么呢？因为，目前针对Matting后面的工作主要集中在合成上面，即将提取好的“优质”的前景图（Foreground Image）与想要进行合成的背景图（Background Image）进行组合，这在图像处理、视频合成等方面都有很重要的前景和意义。

### 简要介绍

$$ I_i = \alpha_iF_i + (1 - \alpha_i)B_i$$

上面的公示(1)是Image Matting问题中很经典的公式，其中$$I$$代表着输入的图像（Input Image），$$F$$代表着前景图（Foreground Image），$$B$$代表着背景图（Background Image），$$\alpha$$代表着前景图的透明度（opacity），$$i$$代表着图像上的第i个像素。

观察上面的公式，我们可以知道公式的左边$$I_i$$的3个数值是已知的（输入图像有RGB三个通道），但是公式右边所有的7个数值是完全未知的（包括前景图的RGB通道、背景图的RGB通道、透明度的数值），注意这些数值都是基于某一个像素点$$i$$的。

现在我们知道了，Image Matting这个问题是相当的ill-posed and underconstrained。想要效果较好地解决这个问题，通常需要用户的交互（User Interaction），现在比较主流的方法就是使用**Trimap**，至于什么是**Trimap**以及如何使用，也会在后面讲到。

> The trimap is a rough (typically hand-drawn) segmentation of the image into three regions: foreground (shown in white), background (shown in black), and unknown (shown in gray).

### 评价指标

* Gradient 主要计算的是预测的alpha matte $$\alpha$$和ground truth $$\alpha_*$$ 的之间的梯度差异

* Connectivity

* SAD（Sum of Absolute Difference）= SAE（Sum of Absolute Error) 即绝对误差和

* MSE（Mean Squared Error）= MSD（Mean Squared Difference）即平均平方误差


### Trimap生成

```python
import cv2
import random
import numpy as np

def gen_trimap(alpha):
    k_size = random.choice(range(2, 5))
    iterations = np.random.randint(5, 15)
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (k_size, k_size))
    dilated = cv2.dilate(alpha, kernel, iterations=iterations)
    eroded = cv2.erode(alpha, kernel, iterations=iterations)
    trimap = np.zeros(alpha.shape)
    trimap.fill(128)
    #trimap[alpha >= 255] = 255
    trimap[eroded >= 255] = 255
    trimap[dilated <= 0] = 0

    ''' 
    alpha_unknown = alpha[trimap == 128]
    num_all = alpha_unknown.size
    num_0 = (alpha_unknown == 0).sum()
    num_1 = (alpha_unknown == 255).sum()
    print("Debug:  0 : {}/{} {:.3f}".format(num_0, num_all, float(num_0)/num_all))
    print("Debug: 255: {}/{} {:.3f}".format(num_1, num_all, float(num_1)/num_all))
    '''

    return trimap
```

### 绝活Composite

```Python
def composite4(fg, bg, a, w, h):
    bg = bg.crop((0,0,w,h))
    bg.paste(fg, mask=a)
    return bg
```

### 随手推荐

git clone太慢怎么办？我其实遇到很多次这样的问题，但是可能之前文件夹也不大，所以也没在意。今天要clone一个500MB的repo，果断去找找办法，不然不知道要下到猴年马月。

这里先提供一下查看repo包size的方法，就是通过GitHub API来访问，它的HTTP格式是这样子的：https://api.github.com/repos/User_Name/Repo_Name。

其中User_Name和Repo_Name是需要用你想要clone的项目所属的用户名和仓库名替换掉的。

举个栗子：https://api.github.com/repos/harreyjia/balabala

在检索有关存储库的信息时，名为size的属性将以整个存储库的大小(包括其所有历史记录)为单位，以KB为单位。

大佬太牛逼了，亲测1.3MB/s的clone速度。https://www.zhihu.com/question/27159393/answer/1117219745

不过问题是git push 该怎么办呢

### 随手记录几个bash command line 的技巧

* ``mkdir -p``就相当于同时创建若干个Directory，例如``mkdir -p A B C``，就相当于在当前目录下同时并列地创建A、B、C三个Directory。

* Shell脚本中常出现的``if [ -f ./file ]``，首先在Bash的官方手册里是这样描述的：

```Shell 
    -f file - True if file exists and is a regular file.
```

所以我们可以清楚地知道``-f``是用来判断``./file``是否存在且是一个常规文件（而不是一个目录等其他类型的文件）。

* 在Shell脚本中想**逐行**读取.txt文件时，可以采用下面这样一种方式读取：即将读取的文件就是``data.txt``，在每读取一行时，就将其在命令行输出。

```Shell
while read p; do
  echo "$p"
done <data.txt
```

* ``os.path.isabs()``查看路径是否存在问题
