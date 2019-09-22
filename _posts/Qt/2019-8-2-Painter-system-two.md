---
title: QPainter绘图系统 - 基本绘图二
tags: [Qt]
---


## 坐标变换函数

坐标变换函数  

* void translate(qreal dx,qreal dy):坐标平移，默认单位是像素
* void rotate(qreal angle)：坐标顺时针旋转，单位是度
* void scale(qreal sx,qreal sy)：参数分别是横纵坐标的缩放比
* void shear(qreal sh,qreal sv)

状态操作：

*   void save()：保存当前坐标状态
*   void restore()：恢复上次保存的坐标状态
*   void resetTransform()：复位所有坐标变换
