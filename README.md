
### 时间戳
#### 代码分析
（1）在布局文件中增加一个TextView来显示时间戳
```Java
<TextView
        android:id="@+id/text2"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:textSize="12dp"
        android:gravity="center_vertical"
        android:paddingLeft="10dip"
        android:singleLine="true"
        android:layout_weight="1"
        android:layout_margin="0dp"
        />
```
（2）数据库中已有文本创建时间和修改时间连个字段，在NodeEditor.java中,找到updateNode()这个函数，选取修改时间这一字段，并将其格式化存入数据库
```Java
Date nowTime = new Date(System.currentTimeMillis());
SimpleDateFormat sdFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String retStrFormatNowDate = sdFormatter.format(nowTime);

values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, retStrFormatNowDate);
```
（3）在NoteList.java的PROJECTION数组中增加该字段的描述，并在SimpleCursorAdapter中的参数viewsIDs和dataColumns增加子段描述，以达到将其读出和显示的目的
```Java
//The columns needed by the cursor adapter
private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
    };
// The names of the cursor columns to display in the view, initialized to the title column
private String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
// The view IDs that will display the cursor columns, initialized to the TextView in
private int[] viewIDs = { R.id.text1,R.id.text2 };

```


### 实验截图

![image](https://github.com/Laihaha/androidwork/blob/master/%E6%88%AA%E5%9B%BE/1.png)

![image](https://github.com/Laihaha/androidwork/blob/master/%E6%88%AA%E5%9B%BE/3.png)
