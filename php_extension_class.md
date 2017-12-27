<h6>本文将记录一下本人第一次如何开发的php扩展类。</h6>
 1.将要创建的类名称为kfc，进入php源码包的ext目录，创建扩展模块骨架：  
 
```
cd ./ext/
./ext_skel --extname=kfc
```
2.更改config.m4，去掉以下三行的dnl注释：   

```
PHP_ARG_WITH(Person, for Person support,
Make sure that the comment is aligned:
[  --with-kfc             Include Person support])
```
3.修改php_kfc.h,在末尾添加kfc类的方法声明：   

```
PHP_METHOD(kfc,__construct);//构造方法
PHP_METHOD(kfc,__destruct);//析构方法
PHP_METHOD(kfc,buy);//析构方法
PHP_METHOD(kfc,eat);//eat方法
PHP_METHOD(kfc,drink);//drink方法
```
4.
