### Sharedpreferences

Sharedpreferences一般用来存放标志性数据及一些设置信息

获取Sharedpreferences的方法:
1. 通过Context创建Sharedpreferences对象

```java
    `Sharedpreferences sp=Context.getSharedPreferences("userinfo",Context.MODE_PRIVATE);`
```

2. 调用Activity对象的getPreferences()方法，获得的Sharedpreferences对象只能在该Activity中使用

****
例：

```java
    Sharedpreferences sp=Context.getSharedPreferences("userinfo",MODE_PRIVATE);
    Editor editor=sp.edit();  //得到一个Editor对象
    editor.putString("username",username);  //往Editor中添加数据
    editor.putString("userage",userage);
    editor.commit();  //提交editor对象
    ----------------------------------------
    //通过Sharedpreferences获取数据
    String username=sp.getString("username","默认名字");
    String userage=sp.getString("userage","默认年龄");
```


****
<br>
Sharedpreferences的四种操作模式
* `Context.MODE_PRIVATE`
* `Context.MODE_APPEND`
* `Context.MODE_WORLD_READABLE`
* `Context.MODE_WORLD_WAITEABLE`

`Context.MODE_APPEND`: 该模式会检查文件是否存在，存在就往文件里追加，否者就创建文件

`Context.MODE_PRIVATE`: 该模式为默认模式，代表该文件为似有数据，只能被应用本身访问，在该模式下，写入的内容会覆盖原文件内容

`Context.MODE_WORLD_WAITEABLE`: 表示当前文件可以被其他应用写入

`Context.MODE_WORLD_READABLE`: 表示当前文件可以被其他应用读取
