### 1. Java程序
##### 1.1 程序的编译和链接
从Java源代码到程序经历编译和链接两个步骤。
1. 编译：将源代码扩展为.class字节码文件，有javac完成。
2. 链接：运行时动态执行，jvm执行Java命令解析字节码文件，转换为二进制代码，然后运行。链接就是根据引用到的类加载相应的字节码并执行。


##### 2. 数据类型
##### 2.1 8种基本数据类型
4种整型：byte/short/int/long
2种浮点型：float/double
字符型：char
布尔型：boolean

|    类型     |    大小      |   取值范围   | 示例|
|:--------| :---------| :------| :----|
|byte|8bit|$(-2^7,2^7-1)$|byte b=10|
|short|16bit|$(-2^{15},2^{15}-1)$|short s=10|
|int|32bit|$(-2^{31},2^{31}-1)$|int i=10|
|long|64bit|$(-2^{63},2^{63}-1)$|long l=10L|
|float|32bit|$(1.4E-45,3.4E+38)$|float f=3.3f|
|double|64bit|$(4.9E-324,1.7E+308)$|double d=3.3d|
|char|16bit||char c='a'|
|boolean|8bit|true,false|boolean b=true|


