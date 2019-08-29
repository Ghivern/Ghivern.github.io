---
title: QPainter--基本绘图一
layout: post
tags: [ Qt ]
---

## 概述

Qpainter绘图系统可以让用户在屏幕或打印设备上使用相同的API进行绘图，基于如下三个类：

*   QPainter：进行绘图操作的类

*   QPainterDevice：供QPainter进行绘图的二维抽象界面

*   QPainterEngine：为QPainter在不同界面进行绘图提供接口，即由QPainter和QPainterDevice内部使用

>注：

1.  一般的绘图界面有QWidget,QPixmap,QImag

2.  Qwidget是常用的绘图设备，从QWidget类继承的类都有paintEvent()响应函数，只要定义并实现`void paintEvent(QPaintEvent *event)`函数，创建一个QPaintet对象获取绘图设备的接口就可以实现在设备上绘图
3.  绘图实例：  
    ```
    //绘制矩形
    void MianWidget::paintEvent(QPaintEvent *event){
        QPainter painter(this);

        int W = this->width();
        int H = this->height();

        QRec rect(W/4,H/4,W/2,H/2);

        QPen pen;
        pen.setBrush(Qt::BrushStyle::Dense3Pattern);
        pen.setStyle(Qt::PenStyle::SolidLine);
        pen.setCapStyle(Qt::PenCapStyle::FlatCap);
        pen.setJoinStyle(Qt::PenJoinStyle::BevelJoin);
        painter.setPen(pen);

        QPixmap pixmap(":images/fox.jpg");

        QBrush brush;
        brush.setColor(Qt::yellow);
        brush.setStyle(Qt::SolidPattern);
        brush.setTexture(pixmap);
        painter.setBrush(brush);

        painter.drawRect(rect);
    }
    ```

## QPainter的属性

*   pen属性：是一个QPen对象，用于控制线条的样式，如线宽、颜色、线型
  
     QPen主要成员函数：
     *  void setWidth(int width)
     *  void setWidthF(qreal width)
     *  void setStyle(Qt::PenStyle style):设置线条样式
     *  void setColor(const QColor &color)：设置线条颜色
     *  void setBrush(const QBrush &brush)：设置线条填充样式
     *  void setCapStyle(Qt::PenCapStyle style)：设置线条端点样式
     *  void setJoinStyle(Qt::PenJoinStyle style)：设置线条连接样式
     * 注：与上述设置属性函数对应均有一个获取属性函数，如`Qt::PenStytle style() const`

*   brush属性：是一个QBrush对象，用于设置一个区域的填充属性
    
    QBrush主要成员函数：
    *   void setColor(const QColor &color)
    *   void setColor(Qt::GlobalColor color)：设置填充颜色
    *   void setStyle(Qt::BrushStyle style)：设置填充样式
    *   void setTexture(const QPixmap &pixmap)：将QPixmap类型图片作为画刷的图片，画刷类型自动设置成Qt::BrushStyle::TexturePattern
    *   void setTextureImage(const QImage &image):将QImage类型图片作为画刷的图片，画刷类型自动设置成Qt::BrushStyle::TexturePattern

    渐变填充：使用渐变类(QGradient的子类)的对象最为Painter的brush
    * QLinearGradient:线性渐变
    * QRadialGradient:辐射渐变
    * QConicalGradient:圆锥形渐变

        QGradient类函数：
        * setSpread(QGradient::Spread method):设置延展方式
        
*   font属性：是一个QFont对象，用于绘制文字时设置文字的属性


## QPainter绘图的基本图形

*   drawImage
*   drawPixmap
*   drawArc
*   drawPath:参数是QPainterPath类对象












