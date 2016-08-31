### SQLite学习

#### SQLite介绍

* SQLite是一款轻型的数据库，是遵守ACID的关联式数据库管理系统，它的设计目标是嵌入式的，而且目前已经在很多嵌入式产品中使用了它，它占用资源非常的低，在嵌入式设备中，可能只需要几百K的内存就够了。它能够支持Windows/Linux/Unix等等主流的操作系统，同时能够跟很多程序语言相结合，比如Tcl、PHP、Java、C++、.Net等，还有ODBC接口，同样比起 Mysql、PostgreSQL这两款开源世界著名的数据库管理系统来讲，它的处理速度比他们都快。

* SQLite和C/S模式的数据库软件不同，它是进程内的数据库引擎，因此不存在数据库的客户端和服务器。使用SQLite一般只需要带上它的一个动态库，就可以享受它的全部功能。而且那个动态库的尺寸也挺小，以版本3.6.11为例，Windows下487KB、Linux下347KB。

* SQLite的核心引擎本身不依赖第三方的软件，使用它也不需要"安装"。数据库中所有的信息（比如表、视图等）都包含在一个文件内。这个文件可以自由复制到其它目录或其它机器上。

#### SQLite数据类型

一般数据采用的固定的静态数据类型，而SQLite采用的是动态数据类型，会根据存入值自动判断。SQLite具有以下五种常用的数据类型：

NULL: 这个值为空值

VARCHAR(n)：长度不固定且其最大长度为 n的字串，n不能超过 4000。

CHAR(n)：长度固定为n的字串，n不能超过 254。

INTEGER: 值被标识为整数,依据值的大小可以依次被存储为1,2,3,4,5,6,7,8.

REAL: 所有值都是浮动的数值,被存储为8字节的IEEE浮动标记序号.

TEXT: 值为文本字符串,使用数据库编码存储(TUTF-8, UTF-16BE or UTF-16-LE).

BLOB: 值是BLOB数据块，以输入的数据格式进行存储。如何输入就如何存储,不改变格式。

DATA ：包含了年份、月份、日期。

TIME：包含了小时、分钟、秒。

#### Android操作SQLite

SQLiteDatabase代表数据库对象，提供了操作数据库的一些方法，在SDK路径下有SQLite3工具，通过它可以创建数据库、创建表及执行一些SQL语句

SQLiteDatabase 常用方法:

    openOrCreateDatabase(String path,SQLiteDatabase.CursorFactory  factory)  //打开或创建数据库

    insert(String table,String nullColumnHack,ContentValues  values)  //插入一条记录

    delete(String table,String whereClause,String[]  whereArgs)  //删除一条记录

    query(String table,String[] columns,String selection,String[]  selectionArgs,String groupBy,String having,String orderBy)  //查询一条记录

    update(String table,ContentValues values,String whereClause,String[] whereArgs)  //修改记录

    execSQL(String sql)  //执行一条SQL语句

    close()  //关闭数据库

#### 创建数据库的两种方法

* 在android中使用SQLiteDatabase的静态方法

    openOrCreateDatabase(String path,CursorFactory factory);
    打开或创建一个数据库，若存在就打开，不存在就创建。

    参数1 数据库创建的路径

    参数2 一般设置为null

* 通过SQLiteOpenHelper,SQLiteOpenHelper是一个辅助类，是抽象类，需复写两个方法:onCreate()、onUpgrade()。创建完帮助类对象，调用getReadableDatabase()方法，帮助创建打开数据库。
例：


    public class MySqliteOpenHelper extends SQLiteOpenHelper{
      public MySqliteOpenHelper(Context context){
        //super(context,name,factory,version);
        //context:  上下文
        //name:  数据库名
        //factory:  用来创建cursor对象，默认为null
        //version:  数据库版本号，从一开始，若发生改变，onUpgrade方法会被调用，android 4.0后只能升不能降
        super(context,"info.db",null,1);

      }

      //onCreate方法会在数据库第一次创建的时候被调用，适合作表结构的初始化，需执行SQL语句，SQLiteDatabase 用来执行SQL语句
      @Override
      public void onCreate(SQLiteDatabase db){
        db.execSQL("create table info (_id integer primary key autoincrement,name varchar(20))");
      }

      //onUpgrade数据库版本号发生改变时才会调用，适合作表的结构的修改
      @Override
      public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion){
        //添加一个字段
        db.execSQL("alter table info phone varchar(11)");
      }
    }

#### 数据库增删查改的两种方法

* 使用SQLiteDatabase对象调用execSQL做增删改,调用rawQuery做查询
,特点是增删改没有返回值,无法判断sql语句是否执行成功

例：

InfoBean.Java

    public class InfoBean{
      public String name;
      public String phone;
    }


---------------------------查询--------------------------

    MySqliteOpenHelper ms=new MySqliteOpenHelper(context);

    public void query(String name){
      SQLiteDatabase db=ms.getReadableDatabase();
      Cursor cursor=db.rawQuery("select _id,name,phone from info where name=?",new String []{name});
      //解析cursor中的数据
      if(cursor!=null && cursor.getCount()>0){  //判断cursor中是否存在数据
        //循环遍历cursor,获取每一行的内容
        while(cursor.moveToNext()){  //游标是否可以移到下一行
          int id=cursor.getInt(0);
          String name=cursor.getString(1);
          String phone=cursor.getString(2);
        }
      }
        cursor.close();
    }

---------------------------添加----------------------------

    public void add(InfoBean bean){
      SQLiteDatabase db=ms.getReadableDatabase();
      //db.execSQL(sql,bindArgs);
      //sql:  执行的sql语句
      //bindArgs:  sql语句中的占位符
      db.execSQL("insert into info(name,phone) value(?,?);"new Object[]{bean.name,bean.phone});
      db.close();      
    }
