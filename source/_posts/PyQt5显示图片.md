---
title: PyQt5显示图片
tag: PyQt5
categories: 2022
---
PyQt5显示图片
<!-- more -->
```
redImg=QImage()
QImage.load(redImg,'path',format='png')
self.label_3.setPixmap(QtGui.QPixmap(redImg))
```

```
img_path='path'
self.showImage = QPixmap(img_path).scaled(self.label_3.width(), self.label_3.height())  # 适应窗口大小
self.label_3.setPixmap(self.showImage)  # 显示图片
```

```python
image=cv2.imread('path')
def showImageRed(self):
	self.image_1 = self.image
	self.image_1 = QtGui.QImage(self.image_1.data, 		self.image_1.shape[1],self.image_1.shape[0],QtGui.QImage.Format_RGB888).rgbSwapped()
	self.label_3.setPixmap(QtGui.QPixmap.fromImage(self.image_1))

   
```

第三种方法可能会有斜影子，修改成下方这样

```
    def showImageRed(self):
        self.image_1 = self.image
        self.image_1 = QtGui.QImage(self.image_1.data, self.image_1.shape[1], self.image_1.shape[0],self.image_1.shape[1]*3, QtGui.QImage.Format_RGB888).rgbSwapped()
        self.label_3.setPixmap(QtGui.QPixmap.fromImage(self.image_1))
```

感觉应该是三通道问题