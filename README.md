
### 时间戳
#### 代码分析
（1）在布局文件中增加一个TextView来显示时间戳
```Java
<RelativeLayout android:layout_height="match_parent"
    android:layout_width="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/listPreferredItemHeight"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
    />
    <TextView
        android:id="@+id/text2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingLeft="5dip"
        android:singleLine="true"
        android:gravity="center_vertical"
    />
</RelativeLayout>

```
（2）在新建笔记和修改笔记中，我们的时间并不是我们习惯的格式，需要我们修改一下时间戳的格式
新建笔记在NotePadProvider中的insert方法，修改笔记在NoteEditor中的updateNote方法，我们需要修改这个方法中的时间戳格式
NotePadProvider中的insert方法:
```Java
Long now = Long.valueOf(System.currentTimeMillis());
//修改 需要将毫秒数转换为时间的形式yy.MM.dd HH:mm:ss
Date date = new Date(now);
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");
String dateFormat = simpleDateFormat.format(date);//转换为yy.MM.dd HH:mm:ss形式的时间
// If the values map doesn't contain the creation date, sets the value to the current time.
if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {
    values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, dateFormat);
}

// If the values map doesn't contain the modification date, sets the value to the current
// time.
if (values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE) == false) {
    values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
}
```
NoteEditor中的updateNote方法:
```Java
long now = System.currentTimeMillis();
Date date = new Date(now);
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");
String dateFormat = simpleDateFormat.format(date);
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
```
（3）NoteList的修改
我们将PROJECTION添加上修改时间：
```Java
private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//添加修改时间
    };

```

PROJECTION只是定义了需要被取出来的数据列，而之后用Cursor进行数据库查询，再之后用Adapter进行装填。
在看完源码之后，Cursor不用变化，我们需要将显示列dataColumns和他们的viewIDs加入修改时间这一属性
加入修改时间后:
```Java
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE }//加入修改时间;
int[] viewIDs = { android.R.id.text1, R.id.text2 }//加入修改时间;

```


### 实验截图

![image](https://github.com/Laihaha/androidwork/blob/master/%E6%88%AA%E5%9B%BE/1.png)

![image](https://github.com/Laihaha/androidwork/blob/master/%E6%88%AA%E5%9B%BE/3.png)
