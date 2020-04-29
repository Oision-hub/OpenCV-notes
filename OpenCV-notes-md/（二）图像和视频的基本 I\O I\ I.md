开门见山，直入正题 🎈

### 处理文件

#### 常用函数介绍

在 OpenCV 中提供了`cv2.imread()`函数用于读取图片和 `cv2.imwrite()`函数用于保存图片。

```python
cv2.imread('imgname.jpg',flag)
cv2.imwrite('imgname.jpg',img)
```

第一个参数是要读取（或输出）图片的文件名，`img`是我们要保存的对象。

参数`flag`介绍🕵️‍♂️：

- IMREAD_UNCHANGED = -1, //!< If set, return the loaded image as is (with alpha channel, otherwise it gets cropped).   
  - flag = -1 -> 8位深度、原通道
- IMREAD_GRAYSCALE = 0,  //!< If set, always convert image to the single channel grayscale image.  
  - flag = 0 -> 8位深度、1通道
- IMREAD_COLOR = 1,  //!< If set, always convert image to the 3 channel BGR color image. 
  - flag = 1 -> 8位深度、3通道
- IMREAD_ANYDEPTH = 2,  //!< If set, return 16-bit/32-bit image when the input has the corresponding depth, otherwise convert it to 8-bit.  
  - flag = 2 -> 原深度、1通道
- IMREAD_ANYCOLOR = 4,  //!< If set, the image is read in any possible color format.  
  - flag = 3 -> 原深度、3通道
- IMREAD_LOAD_GDAL = 8   //!< If set, use the gdal driver for loading the image.   
  - flag = 4 -> 8位深度、3通道

使用`cv2.imshow()`函数在窗口中显示图像。窗口会根据图像大小自动调整。

```python
cv2.imshow('windowsname',img)
```

第一个参数是规定窗口的名称，它是一个字符串。第二个参数是我们要展示的对象。

`imshow`还有一种使用方法是先设置窗口名称再导入图像，方法如下：

```python
cv2.namedWindow('windowsname',cv2.WINDOW_NORMAL)
#若将 cv2.WINDOW_NORMAL 改为 cv2.WINDOW_AUTOSIZE 则创建的窗口大小不可变📌
cv2.imshow('windowsname',img)
```

我们发现在使用`imshow`展示的图片总是一闪而过🎃，解决方法，在`imshow`后加入如下代码：

```python
cv2.waitKey(0) #🏆是它在起作用哦
cv2.destroyAllWindows()
```

- `cv2.waitKey()`等待键盘输入，为毫秒级
- `cv2.destroyAllWindows()`可以删除所有我们建立的窗口

接下来介绍视频及摄像头的操作，和图像类似

```python
#读取视频
import numpy as np
import cv2
video = cv2.VideoCapture('filename.avi')
while(True):
	#逐帧读取
    ret , frame = video.read()
    #逐帧展示 videofile->frame->video
    cv2.imshow('frame',frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):  #按q键退出
    	break
cv2.destroyAllWindows()
```

要读取摄像头的话只需将 filename.avi 换成摄像头引索

```python
#举个例子
import numpy as np
import cv2
video = cv2.VideoCapture(0)  #0📷
while(True):
	#逐帧读取
    ret , img = video.read()
    #逐帧展示 camera->freame->video
    cv2.imshow('Video',img)
    if cv2.waitKey(1) & 0xFF == ord('q'):  #按q键退出
    	break
video.release()  #释放摄像头🎬，别忘了它
cv2.destroyAllWindows()
#运行以上代码就能看见你自己啦😉
```

demo：显示摄像头视频，鼠标点击结束

```python
import cv2

clicked = False
def onMouse(event,x,y,flags,param):
    global clicked
    if event == cv2.EVENT_LBUTTONUP:  #鼠标左键松开
        clicked = True

cameraCapture = cv2.VideoCapture(0)
cv2.namedWindow('ShowMe')  #上面介绍过咯🥳
cv2.setMouseCallback('ShowMe',onMouse)  #获取鼠标输入🖱
print('showing camera feed,click window or press any key to stop')
success,frame = cameraCapture.read()  
#好像忘了介绍😅，read()返回的第一个参数为布尔类型（成功为True,失败为False)，第二个为每一帧的图片
while success and cv2.waitKey(1) == -1 and not clicked:
    cv2.imshow('ShowMe',frame)
    success,frame = cameraCapture.read()

cv2.destroyWindow('ShowMe')
cameraCapture.release()
```

`setMouseCallback()`暂时不展开介绍，后面补充上😶

demo：读取摄像头，录制 10s 的视频并保存

```python
import cv2
import numpy as np

cameraCapture = cv2.VideoCapture(0)
fps = 30  #get()方法无法获得摄像头的准确帧率，需设定！
size = (int(cameraCapture.get(cv2.CAP_PROP_FRAME_WIDTH)),
        int(cameraCapture.get(cv2.CAP_PROP_FRAME_HEIGHT)))  #get()方法获取宽度和高度
videoWriter = cv2.VideoWriter('myVideo.avi',cv2.VideoWriter_fourcc('X','V','I','D'),fps,size)
#cv2.VideoWriter ('vioeoname',视频编解码器,fps,size)
success,frame = cameraCapture.read()
numPramesRemaining = 10 * fps - 1
while success and numPramesRemaining > 0:
    videoWriter.write(frame)  #逐帧写入
    success,frame = cameraCapture.read()
    numPramesRemaining -= 1
cameraCapture.release()  #释放摄像头
```

关于视频编解码器可以查看这篇文章，有很详细的说明：[点击跳转](https://blog.csdn.net/dcrmg/article/details/52215930)

本期就到此结束啦🎏
