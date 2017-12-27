<h6>本文将记录一下本人第一次如何开发的php扩展类。</h6>
 1.将要创建的类名称为kfc，进入php源码包的ext目录，创建扩展模块骨架：  
 
```
cd ./ext/
./ext_skel --extname=kfc
```
2.更改config.m4，去掉以下三行的dnl注释：   

```
PHP_ARG_WITH(kfc, for kfc support,
Make sure that the comment is aligned:
[  --with-kfc             Include kfc support])
```
3.修改php_kfc.h,在末尾添加kfc类的方法声明：   

```
PHP_METHOD(kfc,__construct);//构造方法
PHP_METHOD(kfc,__destruct);//析构方法
PHP_METHOD(kfc,buy);//析构方法
PHP_METHOD(kfc,eat);//eat方法
PHP_METHOD(kfc,drink);//drink方法
```
4.编辑kfc.c,扩展的主要实现都在这个文件里面：
* 首先在kfc.c头部创建一个全局指针 zend_class_entry *kfb_ce;  
```
zend_class_entry *kfb_ce;
```
* 之后修改const zend_function_entry kfc_functions[]函数，这个函数主要是注册kfc类的相关成员方法，如果你写的不是类，是一般函数的话，也是在这里进行注册  
```
const zend_function_entry kfc_functions[] = { 
    PHP_ME(kfc, __construct, NULL, ZEND_ACC_PUBLIC|ZEND_ACC_CTOR)
    PHP_ME(kfc, __destruct,  NULL, ZEND_ACC_PUBLIC|ZEND_ACC_DTOR)
    PHP_ME(kfc, buy,     NULL, ZEND_ACC_PUBLIC)
    PHP_ME(kfc, eat, NULL, ZEND_ACC_PUBLIC)
    PHP_ME(kfc, drink, NULL, ZEND_ACC_PUBLIC)
    PHP_FE_END
};
```
