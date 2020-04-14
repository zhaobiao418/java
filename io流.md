### 其他对象的API

##### System类:

`System类中的字段和方法都是静态的`

`常见方法 : 

ong currentTimeMillis();获取当前时间的毫秒值，可以通过此方法检测程序的执行时间。

---

Properties getProperties();缺点当前的系统属性

Properties集合中存储的都是String类型的键和值

最好使用它自己存储和取出来的方法来完成元素的操作

---

##### Runtime类 

每个java应用程序都有一个Runtime类实例,使应用程序能够与其运行的环境相链接,应用程序不能创建自己的Runtime类实例

runtime : 没有构造方法,说明该类不可以创建对象,并且还有非静态的方法,说明该类应该提供静态的返回该类对象的方法,而且只有一个,说明runtime类使用了单例设计模式

```
01. public class RuntimeDemo{
02. public static void main(String[] args) throws Exception{
03. Runtime r = Runtime.getRuntime();
04.
05. Process p = r.exec( "notepad.exe D:\\code\\day20\\RuntimeDemo.java");
06. Thread.sleep(5000);
07. p.destroy();
08. }
09. }
```

---

##### Math类

Math：提供了操作数学运算的方法，都是静态的。

常用方法：
ceil():返回大于参数的最小整数。
floor():返回小于参数的最大整数。
round():返回四舍五入的整数。
pow(a,b):a的b次方。

----

##### Date、DateFormat类

```
01. import java.util.Date;
02.
03. public class DateDemo{
04. public static void main(String[] args){
05. long time = System.currentTimeMillis();
06. System.out.println(time);
07.
08. //将当前日期和时间封装成Date对象
09. Date date1 = new Date();
10. System.out.println(date1);
11.
12. //将指定毫秒值封装成Date对象
13. Date date2 = new Date(1405244787235l);
14. System.out.println(date2);
15. }
16. }
```

日期对象和毫秒值之间的转换
毫秒值-->日期对象：

1. 通过Date对象的构造方法 new Date(timeMillis);
2. 还可以通过setTime设置。
  因为可以通过Date对象的方法对该日期中的各个字段（年月日等）进行操作。
  日期对象-->毫秒值：
3. getTime方法。
  因为可以通过具体的数值进行运算。
  对日期对象进行格式化：
  将日期对象-->日期格式的字符串。
  使用的是DateFormat类中的format方法。

---

##### Calendar类

Calendar 类是一个抽象类，它为特定瞬间与一组诸如YEAR、MONTH、DAY_OF_MONTH、HOUR
等日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。

----

### IO流

IO流用来处理设备之间的数据传输。Java对数据的操作是通过流的方式。Java用于操作流的对象都在
IO包中。

输入流和输出流相对于内存设备而言

将外设中的数据读取到内存中: 输入

将内存的数写入到外设中 : 输出

流按操作数据分为两种：字节流与字符流。

##### `字符流的由来：

其实就是：字节流读取文字字节数据后，不直接操作而是先查指定的编码表，获取对应的文字。再对
这个文字进行操作。简单说：字节流+编码表。`

##### IO流常用基类-字符流:

字节流的抽象基类：InputStream，OutputStream。

字符流的抽象基类：Reader((读取))，Writer(输出,也叫写入)。

由这四个类派生出来的子类名称都是以其父类名作为子类名的后缀。
如：InputStream的子类FileInputStream。
如：Reader 的子类FileReader。

```
需求：将一些文字存储到硬盘一个文件中。
记住：如果要操作文字数据，建议优先考虑字符流。
而且要将数据从内存写到硬盘上，要使用字符流中的输出流：Writer。
硬盘的数据基本体现是文件，希望找到一个可以操作文件的Writer：FileWriter。
```

IO流的异常处理方式：为防止代码异常导致流无法关闭，因此在finally中对流进行关闭。

---

字符流的缓冲区
缓冲区的出现提高了对数据的读写效率。

对应类：
BufferedWriter
BufferedReader

缓冲区要结合流才可以使用。
作用：在流的基础上对流的功能进行了增强。

字符流缓存区 : 

写入行使用BufferedWriter类中的newLine()方法

读取一行数据使用BufferedReader类中的readLine()方法

原理：使用了读取缓冲区的read方法，将读取到的字符进行缓冲并判断换行标记，将标记前的缓冲数
据变成字符串返回。

---

LineNumberReader
跟踪行号的缓冲字符输入流。此类定义了方法 setLineNumber(int) 和 getLineNumber()，它们可分
别用于设置和获取当前行号。

----

##### 装饰设计模式 ; 对原有类进行了功能的改变,增强

```
01. class Person{
02. void chifan(){
03. System.out.println( "吃饭");
04. }
05. }
06.
07. //采用装饰的方式增强Person类
08. //这个类的出现是为了增强Person而出现的
09. class NewPerson{
10. private Person p;
11.
12. NewPerson(Person p){
13. this.p = p;
14. }
15.
16. public void chifan(){
17. System.out.println( "开胃酒");
18. p.chifan();
19. System.out.println( "甜点");
20. }
21. }
23. //采用继承的方式增强Person类
24. class NewPerson2 extends Person{
25. public void chifan(){
26. System.out.println( "开胃酒");
27. super.chifan();
28. System.out.println( "甜点");
29. }
30. }
31.
32. public class PersonDemo{
33. public static void main(String[] args){
34. Person p = new Person();
35. NewPerson np1 = new NewPerson(p);
36. np1.chifan();
37.
38. System.out.println( "---------------");
39.
40. NewPerson2 np2 = new NewPerson2();
41. np2.chifan();
42. }
43. }
```

装饰和继承都能实现一样的特点：进行功能的扩展增强。有什么区别呢？
首先有一个继承体系：
Writer
|--TextWriter:用于操作文本
|--MediaWriter:用于操作媒体
如果想要对操作的动作进行效率的提高，按照面向对象，可以通过继承的方式对具体的对象进行功能
的扩展，那么就需要加入缓冲技术。
Writer
|--TextWriter:用于操作文本
|--BufferTextWriter:加入了缓冲技术的操作文本的对象
|--MediaWriter:用于操作媒体
|--BufferMediaWriter:加入了缓冲技术的操作媒体的对象
以上方式并不理想，如果这个体系需要再进行功能扩展，又多了更多流对象。
这样就会发现只为提高功能，导致继承体系越来越臃肿，不够灵活。

重新思考问题：
既然加入的都是同一种技术--缓冲。
前一种是让缓冲和自己的流对象相结合。
可不可以将缓冲进行单独的封装，哪个对象需要缓冲就将哪个对象和缓冲关联。

```
01. class Buffer {
02. Buffer(TextWriter w){}
03. Buffer(MediaWriter w){}
04. }
```

简化为 :

```
01.
02. class BufferedWriter extends Writer{
03. BufferedWriter(Writer w){}
04. }
```

Writer
|--TextWriter:用于操作文本
|--MediaWriter:用于操作媒体
|--BufferedWriter:用于提高效率
可见：装饰比继承灵活。
特点：装饰类和被装饰类都必须所属同一个接口或者父类。

分析：
缓冲区中无非就是封装了一个数组，并对外提供了更多的方法对数组进行访问，其实这些方法最终操
作的都是数组的角标。
缓冲的原理：
其实就是从源中获取一批数据到缓冲区中，再从缓冲区中不断地取出一个一个数据。
在此次取完后，再从源中继续取一批数据进缓冲区，当源中的数据取完时，用-1作为结束标记。

```
01. import java.io.FileReader;
02. import java.io.IOException;
03. import java.io.Reader;
04.
05. class MyBufferedReader{
06.
07. private Reader r;
08.
09. //定义一个数组作为缓冲区
10. private char[] buf = new char[1024];
11.
12. //定义一个指针用于操作这个数组中的元素，当操作到最后一个元素后，指针应该归零
13. private int pos = 0;
14.
15. //定义一个计数器用于记录缓冲区中的数据个数，当该数据减到0，就从源中继续获取数据到
缓冲区中
16. private int count = 0;
17.
18. MyBufferedReader(Reader r){
19. this .r = r;
20. }
21.
22. //该方法从缓冲区中一次取一个字符
23. public int myRead() throws IOException {
24. //从源中获取一批数据到缓冲区中，需要先做判断，只有计数器为0时，才需要从源
中获取数据
25. if (count == 0){
26. count = r.read(buf);
27.
28. //每次获取数据到缓冲区后，角标归零
29. pos = 0;
30. }
31.
32. if (count < 0)
33. return -1;
34.
35. char ch = buf[pos];
36.
37. pos++;
38. count--;
39.
40. return ch;
41. }
42.
43. public String myReadLine() throws IOException {
44. StringBuilder sb = new StringBuilder();
45.
46. int ch = 0;
47. while ((ch = myRead()) != -1){
48. if (ch == '\r' )
49. continue ;
50. if (ch == '\n' )
51. return sb.toString();
52. //将从缓冲区读到的字符，存储到缓存行数据的缓冲区中
53. sb.append(( char )ch);
54. }
55.
56. if (sb.length() != 0){
57. return sb.toString();
58. }
59. return null ;
60. }
61.
62. public void myClose() throws IOException {
63. r.close();
64. }
65. }
66.
67. public class MyBufferedReaderDemo{
68. public static void main(String[] args) throws IOException {
69. FileReader fr = new FileReader("buf.txt" );
70.
71. MyBufferedReader bufr = new MyBufferedReader(fr);
72.
73. String line = null ;
74.
75. while ((line = bufr.myReadLine()) != null){
76. System.out.println(line);
77. }
78.
79. bufr.myClose();
80. }
81. }
```

---

##### IO流常用基类-字节流:

基本操作与字符流类相同。但它不仅可以操作字符，还可以操作其他媒体文件

```
01. import java.io.FileOutputStream;
02. import java.io.IOException;
03.
04. public class ByteStreamDemo{
05. public static void main(String[] args) throws IOException {
06. demo_write();
07. }
08.
09. public static void demo_write() throws IOException {
10. //1、创建字节输出流对象，用于操作文件
11. FileOutputStream fos = new FileOutputStream( "bytedemo.txt");
12.
13. //2、写数据，直接写入到了目的地中
14. fos.write( "abcdefg".getBytes());
15.
16. //关闭资源动作要完成
17. fos.close();
18. }
19. }
```

FileOutputStream、FileInputStream的flush方法内容为空，没有任何实现，调用没有意义。
字节流的缓冲区：同样是提高了字节流的读写效率。

##### 转换流:

转换流的由来：
字符流与字节流之间的桥梁
方便了字符流与字节流之间的操作
转换流的应用：
字节流中的数据都是字符时，转成字符流操作更高效。
转换流：
InputStreamReader：字节到字符的桥梁，解码。
OutputStreamWriter：字符到字节的桥梁，编码。
InputStreamReader是字节流通向字符流的桥梁。

使用字节流读取一个中文字符需要读取两次，因为一个中文字符由两个字节组成，而使用字符流只需
读取一次。
System.out的类型是PrintStream，属于OutputStream类别。
OutputStreamReader是字符流通向字节流的桥梁。

---

6 流的操作规律:

之所以要弄清楚这个规律，是因为流对象太多，开发时不知道用哪个对象合适。
想要知道对象的开发时用到哪些对象，只要通过四个明确即可。
1、明确源和目的
源：InputStream Reader
目的：OutputStream Writer
2、明确数据是否是纯文本数据
源：是纯文本：Reader
否：InputStream
目的：是纯文本：Writer
否：OutputStream
到这里，就可以明确需求中具体要使用哪个体系。
3、明确具体的设备
源设备：
硬盘：File
键盘：System.in
内存：数组
网络：Socket流
目的设备：
硬盘：File
控制台：System.out
内存：数组
网络：Socket流
4、是否需要其他额外功能
是否需要高效ÿ缓冲区Ā：

是，就加上buffer

任何Java识别的字符数据使用的都是Unicode码表，但是FileWriter写入本地文件使用的是本地编码，
也就是GBK码表。
而OutputStreamWriter可使用指定的编码将要写入流中的字符编码成字节。

什么时候使用转换流呢？
1、源或者目的对应的设备是字节流，但是操作的却是文本数据，可以使用转换作为桥梁，提高对文本
操作的便捷。
2、一旦操作文本涉及到具体的指定编码表时，必须使用转换流。

![](D:\rj\Program_rj\public\GitHub\depository\zhaobiao418\java\img\Snipaste_2020-04-14_15-23-50.jpg)

![](D:\rj\Program_rj\public\GitHub\depository\zhaobiao418\java\img\Snipaste_2020-04-14_15-25-31.jpg)

---

##### File类:

File类用来将文件或者文件夹封装成对象，方便对文件与文件夹的属性信息进行操作。
File对象可以作为参数传递给流的构造函数。

流只能操作数据，不能操作文件。

```
01. import java.io.File;
02.
03. public class FileDemo{
04. public static void main(String[] args){
05. constructorDemo();
06. }
07.
08. public static void constructorDemo(){
09. //可以将一个已存在的，或者不存在的文件或者目录封装成file对象
10. //方式一
11. File f1 = new File("d:\\demo\\a.txt" );
12.
13. //方式二
14. File f2 = new File("d:\\demo" ,"a.txt" );
15.
16. //方式三
17. File f = new File("d:\\demo" );
18.
19. File f3 = new File(f,"a.txt" );
20.
21. //考虑到Windows和Linux系统通用
22. File f4 = new File("d:" + File.separator + "demo" +
File.separator + "a.txt" );
23. }
24. }
```

File.separator是与系统有关的默认名称分隔符。在 UNIX 系统上，此字段的值为 '/'；在 Microsoft
Windows 系统上，它为 "\\\"。

File对象的常用方法：
1、获取
获取文件名称
获取文件路径
获取文件大小
获取文件修改时间

```
01. import java.io.File;
02. import java.text.DateFormat;
03. import java.util.Date;
04.
05. public class FileMethodDemo{
06. public static void main(String[] args){
07. getDemo();
08. }
09.
10. public static void getDemo(){
11. File file1 = new File("a.txt" );
12. File file2 = new File("d:\\demo\\a.txt" );
13.
14. String name = file2.getName();
15. String absPath = file2.getAbsolutePath(); //绝对路径
16. String path1 = file1.getPath();
17. String path2 = file2.getPath();
18. long len = file2.length();
19. long time = file2.lastModified();
20. //相对路径不同，父目录不同
21. //如果此路径名没有指定父目录，则返回 null
22. String parent1 = file1.getParent();
23. String parent2 = file2.getParent();
24.
25. Date date = new Date(time);
26. DateFormat df =
DateFormat.getDateTimeInstance(DateFormat.LONG,DateFormat.LONG);
27. String str_time = df.format(date);
28.
29. System.out.println( "name:" + name);
30. System.out.println( "absPath:" + absPath);
31. System.out.println( "path1:" + path1);
32. System.out.println( "path2:" + path2);
33. System.out.println( "len:" + len);
34. System.out.println( "str_time:" + str_time);
35. System.out.println( "parent1:" + parent1);
36. System.out.println( "parent2:" + parent2);
37. }
38. }
```

2、创建和删除

```
01. import java.io.File;
02. import java.io.IOException;
03.
04. public class FileMethodDemo{
05. public static void main(String[] args) throws IOException {
06. createAndDeleteDemo();
07. }
08.
09. public static void createAndDeleteDemo() throws IOException {
10. File file = new File("file.txt" );
11.
12. //和输出流不一样，如果文件不存在，则创建，如果文件存在，则不创建
13. boolean b1 = file.createNewFile();
14. System.out.println( "b1 = " + b1);
15.
16. //使用delete方法删除文件夹的时候，如果文件夹中有文件，则会删除失败
17. boolean b2 = file.delete();
18. System.out.println( "b2 = " + b2);
19.
20. File dir = new File("abc\\ab" );
21. //使用mkdir可以创建多级目录
22. boolean b3 = dir.mkdir();//make directory
23. System.out.println( "b3 = " + b3);
24.
25. //最里层目录被干掉，dir代表的是最里层的目录
26. boolean b4 = dir.delete();
27. }
28. }
```

3、判断

```
01. import java.io.File;
02. import java.io.IOException;
03.
04. public class FileMethodDemo{
05. public static void main(String[] args) throws IOException {
06. isDemo();
07. }
09. public static void isDemo() throws IOException {
10. File f = new File("aaa.txt" );
11.
12. boolean b = f.exists();
13. System.out.println( "b = " + b);
14.
15. if(!f.exists()){
16. f.createNewFile();
17. }
18.
19. //最好先判断是否存在
20. if(f.exists()){
21. System.out.println(f.isFile());
22. System.out.println(f.isDirectory());
23. }
24.
25. f = new File("aa\\bb" );
26.
27. f.mkdirs();
28. if(f.exists()){
29. System.out.println(f.isFile());
30. System.out.println(f.isDirectory());
31. }
32. }
33. }
```

4、重命名

```
01. import java.io.File;
02. import java.io.IOException;
03.
04. public class FileMethodDemo{
05. public static void main(String[] args) throws IOException {
06. renameToDemo();
07. }
08.
09. public static void renameToDemo() throws IOException {
10. File f1 = new File("d:\\code\\day21\\0.mp3" );
11. File f2 = new File("d:\\code\\day21\\1.mp3" );
12.
13. boolean b = f1.renameTo(f2);
14.
15. System.out.println( "b = " + b);
16. }
17. }
```

5、系统根目录和容量获取

```
01. import java.io.File;
02. import java.io.IOException;
03.
04. public class FileMethodDemo{
05. public static void main(String[] args) throws IOException {
06. listRootsDemo();
07. }
08.
09. public static void listRootsDemo() throws IOException {
10. File[] files = File.listRoots();
11.
12. for(File file : files){
13. System.out.println(file);
14. }
15.
16. File file = new File("d:\\" );
17.
18. System.out.println( "getFreeSpace:" + file.getFreeSpace());
19. System.out.println( "getTotalSpace:" + file.getTotalSpace());
20. System.out.println( "getUsableSpace:" + file.getUsableSpace());
21. }
22. }
```

需求：获取目录下的文件以及文件夹的名称。

```
01. import java.io.File;
02. import java.io.IOException;
03.
04. public class FileListDemo{
05. public static void main(String[] args) throws IOException {
06. listDemo();
07. }
08.
09. public static void listDemo() throws IOException {
10. File file = new File("c:\\" );
11.
12. //获取目录下的文件以及文件夹的名称，包含隐藏文件
13. //调用list方法的File对象中封装的必须是目录，否则会产生NullPointerException
14. //如果访问的是系统级目录也会发生空指针异常
15. //如果目录存在但是没有内容，会返回一个数组，但是长度为0
16. String[] names = file.list();
17.
18. for(String name : names){
19. System.out.println(name);
20. }
21. }
22. }
```

----

##### File类

需求：获取c盘目录下的隐藏文件。

```
01. import java.io.File;
02. import java.io.FilenameFilter;
03.
04. public class FileListDemo{
05. public static void main(String[] args){
06. listDemo();
07. }
08.
09. public static void listDemo(){
10. File dir = new File("c:\\" );
11.
12. File[] files = dir.listFiles( new FilterByHidden());
13.
14. for(File file : files){
15. System.out.println(file);
16. }
17. }
18. }
19.
20. class FilterByHidden implements FilenameFilter{
21. public boolean accept(File dir,String name){
22. return dir.isHidden();
23. }
24. }
```

##### 递归:

函数自身直接或者间接的调用到了自身。
一个功能在被重复使用，并每次使用时，参与运算的结果和上一次调用有关。这时可以用递归来解决
问题。
P.S.
1、递归一定明确条件，否则容易栈溢出。
2、注意一下递归的次数。
需求：对指定目录进行所有内容的列出（包含子目录中的内容），也可以理解为深度遍历。

```
01. import java.io.File;
02.
03. public class FileListDemo{
04. public static void main(String[] args){
05. File dir = new File("D:\\Java\\jdk1.6.0_02\\include" );
06. listAll(dir,0);
07. }
08.
09. public static void listAll(File dir, int level){
10. System.out.println(getSpace(level) + "dir:" +
dir.getAbsolutePath());
11. //获取指定目录下当前的所有文件夹或者文件对象
12. level++;
13. File[] files = dir.listFiles();
14.
15. for (int x = 0; x < files.length; x++){
16. if (files[x].isDirectory()){
17. listAll(files[x],level);
18. }
19. System.out.println(getSpace(level) + "file:" +
files[x].getAbsolutePath());
20. }
21. }
22.
23. private static String getSpace( int level){
24. StringBuilder sb = new StringBuilder();
25.
26. sb.append( "|--" );
27. for (int x = 0; x < level; x++){
28. sb.append( "| " );
29. }
30.
31. return sb.toString();
32. }
33. }
```



-----

##### Properties集合

Map
|--Hashtable
|--Properties
特点：

1. 该集合中的键和值都是字符串类型。
2. 集合中的数据可以保存到流中，或者从流中获取。
3. 通常该集合用于操作以键值对形式存在的配置文件。
  Properties集合的存和取。

---

##### IO包中的其他类:

PrintWriter与PrintStream：可以直接操作输入流和文件。
PrintStream为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式。
与其他输出流不同，PrintStream永远不会抛出IOException。
PrintStream打印的所有字符都使用平台的默认字符编码转换为字节。
在需要写入字符而不是写入字节的情况下，应该使用PrintWriter类。
PrintStream:

1. 提供了打印方法可以对多种数据类型值进行打印，并保持数据的表示形式
2. 它不抛IOException
  构造函数，接收三种类型的值：
3. 字符串路径
4. File对象
5. 字节输出流
  示例1：
6. import java.io.PrintStream

---

PrintWriter：字符打印流
构造函数参数：

1. 字符串路径
2. File对象
3. 字节输出流
4. 字符输出流

----

##### IO包中的其他类:

操作对象
ObjectInputStream与ObjectOutputStream
P.S.
被操作的对象需要实现Serializable。
类通过实现java.io.Serializable接口以启用序列化功能，Serializable只是一个标记接口。

RandomAccessFile
随机访问文件，自身具备读写的方法。
通过skipBytes(int x),seek(int x)等方法来达到随机访问。
特点：

1. 该对象即能读，又能写。
2. 该对象内部维护了一个byte数组，并通过指针可以操作数组中的元素。
3. 可以通过getFilePointer方法获取指针的位置，和通过seek方法设置指针的位置。
4. 其实该对象就是将字节输入流和输出流进行了封装。
5. 该对象的源或者目的只能是文件。通过构造函数就可以看出。
  P.S.
  RandomAccessFile不是io体系中的子类。