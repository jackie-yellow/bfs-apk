1. 在应用的 Build.gradle 配置
```kotlin
buildscript {
    repositories {
        ...
    }
    dependencies {
        ...
        classpath("com.google.protobuf:protobuf-gradle-plugin:0.8.19")
    }

}
```
2. 在APP的build.gradle配置
```kotlin
plugins {
    ...
    id("com.google.protobuf")//.version("0.8.19")
}

dependencies {
    ...
    //implementation("androidx.datastore:datastore-preferences:1.1.0-alpha01") 
    //implementation("com.google.protobuf:protobuf-javalite:3.19.2") 
    implementation("com.google.protobuf:protobuf-java:3.19.2") 
}

protobuf {
    val configuration = this
    configuration.protoc {
        artifact = "com.google.protobuf:protoc:3.19.2"
    }

    configuration.generateProtoTasks {
        this.all().forEach { task ->
            task.builtins {
                id("java") {
                    java { }
                }
            }
        }
    }
}

// 不需要在android sourceSet中添加 srcDir

```
3. 新建proto： app/src/main/proto，运行是会自动解析该目录下的文件 xxx.proto  
```kotlin
//指定 Protobuf 版本
syntax = "proto3";

//指定包名
package com.exmple.protobuf;

//定义一个学生的消息类
message Student{
  //姓名
  string name = 1;
  //年龄
  int32 age = 2;
  //邮箱
  string email = 3;
  //课程
  repeated string course = 4;

}

//定义一个天气的消息类
message Weather{
  int32 query = 1;

  //季节
  enum Season{
    option allow_alias = true;
    //春
    SPRING = 0;
    //夏
    SUMMER = 1;
    //秋
    FALL = 2;
    AUTUMN = 2;
    //冬
    WINTER = 3;
  }

  Season season = 2;
}
```

4. 生成文件在：app/build/generate/source/proto/debug

## 关键字内容：
关键字 | 说明
--|--
syntax  |  指定 Protobuf 的版本，Protobuf 目前有 proto2 和 proto3 两个常用版本，如果没有声明，则默认是proto2
package  |  指定文件包名
import  |  导包，和 Java 的 import 类似
message  |  定义消息类，和 Java 的 class 关键字类似，消息类之间可以嵌套
repeated  |  定义一个集合，和 Java 的集合类似
reserved  |  保留字段，如果使用了这个关键字修饰，用户就不能使用这个字段编号或字段名
option  |  option 可以用在 Protobuf 的 scope 中，或者 message、enum、service 的定义中，Protobuf 定义的 option 有 java_package，java_outer_classname，java_multiple_files 等等
optional  |  表示该字段是可选的
java_package  |  指定生成类所在的包名，需配合
java_outer_classname  |  定义当前文件的类名，如果没有定义，则默认为文件的首字母大写名称
java_multiple_files  |  指定编译过后 Java 的文件个数，如果是 true，那么将会一个 J

<p>

## Protobuf 基本数据类型
Protobuf Type  |  说明  |  对应 Java/Kotlin Type
--  | -- | --
double  |  固定 8 字节长度  |  double
float  |  固定 4 字节长度  |  float
int32  |  可变长度编码，对负数编码低效，如果字段可能为负数，用 sint32 代替  |  int
int64  |  可变长度编码，对负数编码低效，如果字段可能为负数，用 sint64 代替  |  long
uint32  |  可变长度编码，无符号整数  |  int
uint64  |  可变长度编码，无符号整数  |  long
sint32  |  可变长度编码，有符号整数  |  int
sint64  |  可变长度编码，有符号整数  |  long
fixed32  |  固定 4 字节长度，无符号整数  |  int
fixed64  |  固定 8 字节长度，无符号整数  |  long
sfixed32  |  固定 4 字节长度，有符号整数  |  int
sfixed64  |  固定 8 字节长度，有符号整数  |  long
bool  |  布尔类型，值为 true 或 false  |  boolean
string  |  字符串类型  |  String
