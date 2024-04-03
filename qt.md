#### QT学习



**QMainWindow、QWidget、QDialog区别**

QMainWindow 类提供一个有菜单条、锚接窗口（例如工具条）和一个状态条的主应用程序窗口。 主窗口通常用在提供一个大的中央窗口部件（例如文本编辑或者绘制画布）以及周围菜单、工具条和一个状态条。QMainWindow常常被继承，因为这使得封装中央部件、菜单和工具条以及窗口状态条变得更容易，当用户点击菜单项或者工具条按钮时，槽会被调用。

QWidget类是所有用户界面对象的基类。 窗口部件是用户界面的一个基本单元：它从窗口系统接收鼠标、键盘和其它事件，并且在屏幕上绘制自己。每一个窗口部件都是矩形的，并且它们按Z轴顺序排列。一个窗口部件可以被它的父窗口部件或者它前面的窗口部件盖住一部分。

QDialog 是最普通的顶级窗口。 一个不会被嵌入到父窗口部件的窗口部件叫做顶级窗口部件。通常情况下，顶级窗口部件是有框架和标题栏的窗口（尽管使用了一定的窗口部件标记，创建顶级窗口部件时也可能没有这些装饰。）在Qt中，QMainWindow和不同的QDialog的子类是最普通的顶级窗口



 创建类的基类：带菜单的窗口（QMainWindow ），**空白窗口（QWidget）**，对话框窗口（QDialog ）



![image-20240402150644663](assets/image-20240402150644663.png)



##### QT项目框架及文件介绍



QT项目中的main函数

```c++
#include "widget.h"//QT中一个类对应一个头文件，类名就是头文件名

#include <QApplication>//QT系统提供的标准类名头文件

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);//应用程序类(整个后台管理的命脉。处理应用程序的初始化和结束，事件处理调度，仅存在一个)
    Widget w;//实例化对象，调用构造函数
    w.show();//显示图形化界面


    return a.exec();//主事件循环，在exec函数中
}

```



##### 设置窗口属性

1.确定代码书写位置

.pro:用于生成可执行文件

mian.cpp:主函数

**widget.cpp:类的函数（一般在这写）**

​	一般情况，窗口的属性和添加控件和对控件的操作都会在类的构造函数中书写

​	

widget.h:类和头文件



2.设置窗口

```c++
Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
    //修改窗口标题
    this->setWindowTitle("hello QT");
    
    //设置窗口大小，设置完成拉伸
    //this->resize(800, 800);
    
    //设置固定大小，不可拉伸
    this->setFixedSize(500, 500);
}
```



##### 设置按钮

```c++
1.需要包含头文件 
#include "QPushButton"

2.qmake中添加QT += widgets
```

```c++
Widget::Widget(QWidget *parent)
    : QWidget(parent)
{

    //创建按钮	方式1
    QPushButton *button = new QPushButton;

    //另开一个窗口显示按钮
    //button->show();

    //设置按钮的父对象为窗口
    button->setParent(this);

    //设置按钮属性
    button->setText("按钮1");

    //设置按钮显示位置
    button->move(200,200);

    //设置按钮大小
    button->setFixedSize(100,100);
    
    //方式2
    QPushButton *button2 = new QPushButton("按钮二", this);
    
}
```

二者区别：

方式1：窗口默认大小，按钮在左上角

方式2：窗口是根据按钮的大小来创建的



对象树（对象模型）

概念：QT对象间的父子关系

一定程度上解决了内存问题。简化内存回收





##### 信号与槽机制

![image-20240403154326544](assets/image-20240403154326544.png)

松散耦合，信号发出端和接受端无关联，如需要关联需要使用connect函数



connect函数 + 系统自带的信号和槽（Slots）

![image-20240403154853404](assets/image-20240403154853404.png)

事件源 事件 事件订阅者 事件处理函数

#### 

关闭窗口

```c++
Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
    //创建按钮
    QPushButton *button = new QPushButton("点击关闭窗口",this);
    this->resize(600,400);

    //信号与槽
    //connect(sender, signal, receiver, solt)
    //事件源 事件 事件订阅者 事件处理函数
    connect(button, &QPushButton::clicked, this, &QWidget::close);
}
```



自定义信号与槽

1.确定场景

2.添加相关类

3.声明信号，声明槽函数并实现

信号：一般卸载signals中，返回类型是void，参数可以存在，可以仅声明不需要实现，可以重载

槽函数：一般在public slots里面写，返回值为void，参数可以存在，需要声明并且实现，可以重载

4.创建对象+信号连接

5.触发信号（emit关键字）

在QT中，`emit` 是一个关键字，用于在对象之间发送信号。当一个类中定义了一个信号，并且我们希望在某个特定的时刻发出这个信号时，就可以使用 `emit` 关键字。这将触发连接到该信号的槽函数，从而实现对象之间的通信和交互。 

```c++
	this->teacher = new Teacher();
    this->student = new Student();

    connect(teacher, &Teacher::hungery, student, &Student::treat);

    //触发信号
    ClassOver();
```

