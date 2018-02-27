# 也是遇到一个神奇的需求,需要选择一周内不同的时间段,且时间不能重合
![IMG_2143584E108B-1.jpeg](http://upload-images.jianshu.io/upload_images/3258209-0a73d014e00d314f.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/400) 
# 我们先来解释一下单个时间段如何比较,然后在解释不同星期下如何对比
#### 单个重叠的时间无非下列几种情况:
* 第一种:
![Snip20180227_11.png](http://upload-images.jianshu.io/upload_images/3258209-2cec74c1425bc936.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)
* 第二种:
![Snip20180227_12.png](http://upload-images.jianshu.io/upload_images/3258209-1d7f72693bac7fc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)
* 第三种:
![Snip20180227_13.png](http://upload-images.jianshu.io/upload_images/3258209-7478f837915fb677.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)
* 第四种(其实和第一种一样):
![Snip20180227_14.png](http://upload-images.jianshu.io/upload_images/3258209-c01089ab0e0142a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)
从图中可 要判断`时间段1`是否存在`时间段2`,只需要判断时间开头和结尾是否存在另一个时间段中,也就是S2<S1<E2 OR S2<E< E2,但是还存在一种情况就是图4展示的,所以还要判断 S1<S2<E1 OR S1<E2<E1
伪代码
```
        if startTime1 >= startTime2 && startTime1 <= endTime2 {
            return true
        }else if endTime1 >= startTime2 && endTime1 <= endTime2 {
            return true
        }else if startTime2 >= startTime1 && startTime2 <= endTime1 {
            return true
        }else if endTime2 > startTime1 && endTime2 <= endTime1 {
            return true
        }
```
接下来说说如何判断星期内如何重叠,其实道理是一样的,只是拉长了时间段
如下图:![Snip20180227_15.png](http://upload-images.jianshu.io/upload_images/3258209-ad00ef38d8313094.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)
其实可以把每个时间段分割开来
每分钟60秒
每小时3600秒
每天86400秒
周日~周一这段时间S=0 E=86400
周一~周二 S=86400 E=86400
以此类推
![Snip20180227_18.png](http://upload-images.jianshu.io/upload_images/3258209-00af3093ebb0ff91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)
那么我们就是能知道上图的时间1(红色时间段)的时间理解为:
S = 1*86400 + 开始时间
E = 1*86400 + 结束时间
时间2(蓝色时间段)的时间为:
S = 3*86400 + 开始时间
E = 4*86400 + 结束时间
这样就能计算出时间段在一周内的范围
## 但是还有一个问题,如果超过周六到了下周如何计算
如下图
![Snip20180227_19.png](http://upload-images.jianshu.io/upload_images/3258209-bd626959fa563952.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
这种情况,我们也可以进行切分,分成图中的前段时间和后段时间
首先判断是否超过本周
因为我们将每周分割成了7段,每段86400
那么只要结束时间超过了7*86400,我们就可以定义为周日的时间
前段时间:
S = 6*86400 + 开始时间
E = 7 * 86400
后段时间:
S = 0
E = 结束时间 - 7*86400
这样就获取到了两个时间段
虽然不难,也希望对有相同需求的朋友们有所帮助
github地址:https://github.com/CZXBigBrother


