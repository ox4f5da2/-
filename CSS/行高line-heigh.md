## 行高line-heigh

### 什么是line-height

是两行文字的基线之间的距离，入下图所示

![]( https://img-blog.csdnimg.cn/20191119160625194.png)

![](https://img-blog.csdnimg.cn/20191119160751294.png)

> 改变line-height属性的值，会导致半行距(文字上下的间距)也跟着改变，因为行距=line-height和font-size之差



### line-height赋值

1. normal：使用浏览器的默认值，但是每个浏览器默认值不一样
2. 纯数字：数字会与当前的字体尺寸相乘来设置行间距，即number为当前font-size的倍数
3. 百分比：基于当前字体尺寸的百分比行间距
4. 长度：与纯数字相比带有px单位，设置固定的行间距