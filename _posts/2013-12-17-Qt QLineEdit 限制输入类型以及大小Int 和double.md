---
layout: post
comments: true
categories: QT编程
---

转自[CSDN](http://blog.csdn.net/a_sungirl/article/details/17373405)

1.QLineEdit 限制整数

    m_LineEditIterate = new QLineEdit();  
    m_LineEditIterate->setFixedWidth(100);  
    m_LineEditIterate->setSizePolicy(QSizePolicy::Fixed, QSizePolicy::Fixed);  
    m_LineEditIterate->setText("2");  
    m_LineEditIterate->setValidator(new QIntValidator(1, 9, m_LineEditIterate));  

2.QLieEdit限制double类型,以及小数点,{0,2}这个是控制位数,

    QRegExp double_rx100("100|([0-9]{0,2}[\.][0-9]{1,3})");   
    QRegExp double_rx10000("10000|([0-9]{0,4}[\.][0-9]{1,3})");   
    QRegExp double_rx10("10|([0-9]{0,1}[\.][0-9]{1,3})");   
    m_LineEditDensity = new QLineEdit;  
    m_LineEditDensity->setValidator(new QRegExpValidator(double_rx100, m_LineEditDensity));  
    m_LineEditParA = new QLineEdit;  
    m_LineEditParA->setValidator(new QRegExpValidator(double_rx10000, m_LineEditParA));  
    m_LineEditParB = new QLineEdit;  
    m_LineEditParB->setValidator(new QRegExpValidator(double_rx10, m_LineEditParB));  
3.QLineEdit只输入字母和数字

    QRegExp regx("[a-zA-Z0-9]+$");  
    QValidator *validator = new QRegExpValidator(regx, lined );  
    lined->setValidator( validator );  
