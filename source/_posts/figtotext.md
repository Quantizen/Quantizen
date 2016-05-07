title: 圖片文字
date: 2016-03-28 10:54:41
categories: code
tags: fun
---

>有趣，即最大的才情

<!--more-->

见有人这么做, 细想如下几步即能实现:

1. 用ImageData提取出每个像素点的RGB数据;
2. 在每个像素点处从一个字典中生成文字;
3. 将文字和RGB数据对应而后作图;
4. 若原始图像较大, 可先用Rasterize处理;
5. 注意用ImageReflect和Transpose调整位置.

```mathematica
(*'fig' is the figure file, 'time' is the step of the coarsed data, 'alpha' is the text content*)
figtotxt[fig_, time_, alpha_] := Module[{i, mat, alphabet, dim},
  i = ImageReflect@fig;
  (*i = Rasterize[ImageReflect@fig,20];*)  
  mat = Transpose@ImageData[i];
  alphabet = Characters[alpha];
  dim = Dimensions[mat, 2];
  Graphics[
   Table[{RGBColor[Sequence @@ (mat[[time*i, time*j]])], 
     Text[RandomChoice[alphabet], {time*i, time*j}]}, {i, 
     IntegerPart[dim[[1]]/time]}, {j, IntegerPart[dim[[2]]/time]}]]]
```

一些處理效果爲

![圖片文字](http://7rflrv.com1.z0.glb.clouddn.com/figtotext.jpg)