---
layout: post
title: java question
categories: study
tags : 
toc: true

---

# `java.io.StreamCorruptedException: invalid type code: AC`错误的解决方法
>在向一个文件写入可序列化对象时，每次只想向文件的末尾添加一个可序列化的对象，于是使用了FileOutputStream（文件名，true）间接的构建了ObjectOutputStream流对象，在向外读数据的时候第一次运行的时候不会报错，在第二次就会报java.io.StreamCorruptedException: invalid type code: AC错误

在一个文件都有一个文件的头部和文件体。由于对多次使用`FileOutputStream`（文件名，true）构建的ObjectOutputStream对象向同一个文件写数据，在每次些数据的时候他都会向这个文件末尾先写入header在写入你要写的对象数据，在读取的时候遇到这个在文件体中的header就会报错。导致读出时，出现`streamcorrput`异常（流中的控制信息不一致）。 
只有续写会先写入header，不是续写的话，写入多个对象也只会写入一次文件头，在一次续写时即使写入多个对象，也只会写入一次文件头

知识储备： 
每个文件都有文件的头部和文件体两部分 
在对象输出流（ObjectOutputStream）中有Protected方法writeStreamHeader():void，这个方法是专门来写文件的头部了。

## 解决方法
所以这里要判断是不是第一次写文件，若是则写入头部，否则不写
```java

import java.io.*

public class MyObjectOutputStream extends ObjectOutputStream {
    public MyFileOutputStream() {
        super();
    }

    public MyFileOutputStream(OutputStream o) {
        super(o);
    }

    public void writeStreamHeader() {
    }//这里覆写父类中的方法，使他调用writeObject()的时候不写入文件头
}

class Demo {
    public static void main(String[] args) {
        File file = new File(xxxxxxx);
        ObjectOutputStream out;
        //判断文件是否存在，以决定要使用哪个objectoutputstream
        if (file.isFile()) {
            out = new MyObjectOutputStream(xxxx);
        } else {
            out = new ObjectOutputStream(xxxxx);
        }
    }
}
```
在循环，或者线程里面每次都要new一个新的输出流对象，尤其是ObjectOutputStram是基于其他OutPutStream转换过来的时候


# mysql的DateTime类型
使用java.sql.TimeStamp接受

# mysql列名和java字段不一致
mysql映射到对象会根据set字段名去匹配
SQL语句调整

```shell
select name as personName;
```