#### Object类:所有类的根类

```
具备所有对象的共性内容(意思是它的属性和方法,所有类都可以用,所有的类默认继承了Object)

object的equals函数方法: 默认根据对象的哈希值判断来个对象是否相等

也可以通过object的equals方法来重写比较规则

```

`Object类的toString方法默认返回的内容是“对象所属的类名+@+对象的哈希值（十六进制）”。`

#### 可以查看java.api擦看object的全部方法内容

----

##### 包 

对类文件进行分类管理,给类提高多次命名空间(意思是好几个文件夹包都是子孙关系,里面放着class文件)

<u>类名的全称</u> : 包名(文件夹包,也称为目录).类名

包也是一种封装形式

`包与包之间的类进行访问，被访问的包中的类必须是public的，被访问的包中的类的方法也必须是public的。`

`包之间的访问：被访问的包中的类权限必须是public的。
类中的成员权限：public或者protected。
protected是为其他包中的子类提供的一种权限。`

mport
一个程序文件中只有一个package，但可以有多个import

import: 用到导入需要调用的类的包名: 意思是可以找到调用的包,否则会调用失败,或者调用的对象nue完整的全名

(全名指: 所有的文件夹包名././java文件对象();)而java文件名,就是对象名也是类名

import也可以导入调用的类名

例如:`import org.it315.example.TestStaticImport;`

---

##### jar包的理解

就是一些类文件的压缩成jar的形式,或者叫打成jar包,在我们的java项目中经常导入jar包,因为需要一些框架,而框架的核心类库就是从jar包里面出来的,同时jar包里的类和对象方法,供我们使用和调用

方便于使用，只要在classpath设置jar路径即可。

数据库驱动，SSH框架等都是以jar包体现的。

Jar包的操作：

通过jar.exe工具对jar的操作

```
创建jar包
jar -cvf mypack.jar packa packb
查看jar包
jar -tvf mypack.jar [>定向文件]
解压缩
jar -xvf mypack.jar
自定义jar包的清单文件
jar –cvfm mypack.jar mf.txt packa packb
```

