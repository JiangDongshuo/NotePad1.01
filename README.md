# 期中作业
添加了两项基本功能：显示时间和搜索</br>
添加了三项附加功能：美化UI,更改背景颜色和文件导出</br>
# 显示时间
首先修改NoteEditor类，使文件创建时存储的时间格式，以正常的形式代替毫秒，核心代码如下：</br>
Date date = new Date();</br>
DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");</br>
String time = format.format(date);</br>
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, time);</br>
</br>
其次修改NoteList类，使列表读取时读出时间，并使适配器载入自己编写的view里，核心代码如下：</br>
super.onCreate(savedInstanceState);</br>
setContentView(R.layout.listback);//加入自己设置的背景layout为后续搜索做准备</br>
</br>
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;//增加一条搜索的项目，时间</br>
        int[] viewIDs = { R.id.head1,R.id.time1 };</br>
        SimpleCursorAdapter adapter</br>
            = new SimpleCursorAdapter(</br>
                      this,</br>
                      R.layout.newnoteslist_item,</br>
                      cursor,</br>
                      dataColumns,</br>
                      viewIDs</br>
              );</br>
        ListView listView=(ListView) findViewById(android.R.id.list);</br>
listView.setAdapter(adapter);//将适配器载入自己编写的view中</br>
</br>
效果截图如下：</br>
首先创建一个新的文档</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/2.png)</br>
回到列表显示界面看到时间</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/3.png)</br>
</br>
# 模糊实时搜索笔记本
之前创建的适配器背景中加入了一个置顶的文本编辑框用于搜索</br>
监听文本框变化，如有变化重新加载适配器并加入搜索条件，核心代码如下：</br>
editText.addTextChangedListener(new TextWatcher() {//监听文本框变化</br>
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {</br>
            }</br>
            public void onTextChanged(CharSequence s, int start, int before, int count) {</br>
                System.out.println("搜索框文本变化了");</br>
                System.out.println(editText.getText().toString());//获取文本框内容</br>
                String [] searchContent={editText.getText().toString()+"%"};//设置搜索条件</br>
                Cursor cursor = managedQuery(</br>
                        getIntent().getData(),            </br>
                        PROJECTION,                       </br>
                        "title like ?",                             // 设置搜索条件</br>
                        searchContent,                             </br>
                        NotePad.Notes.DEFAULT_SORT_ORDER  </br>
                );</br>
                ListView listView=(ListView) findViewById(android.R.id.list);</br>
                String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;</br>
                int[] viewIDs = { R.id.head1,R.id.time1 };</br>
                SimpleCursorAdapter adapter=new SimpleCursorAdapter(</br>
                       listView.getContext() ,R.layout.newnoteslist_item,cursor,dataColumns,viewIDs</br>
                );</br>
                listView.setAdapter(adapter);//重新载入适配器</br>
            }</br>
</br>
效果截图如下：</br>
输入tes会将所有包含tes标题的笔记实时搜索出来</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/4.png)</br>
输入test1会将所有包含test1标题的笔记实时搜索出来</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/5.png)</br>
</br>
# 修改背景颜色
在menu加入一个group，并增加对group内item的监听，对view的背景色进行改变，核心代码如下：</br>
            case R.id.color_white:</br>
                noteEditor.setBackgroundColor(Color.WHITE);</br>
                break;</br>
            case R.id.color_gray:</br>
                noteEditor.setBackgroundColor(Color.GRAY);</br>
                break;</br>
            case R.id.color_blue:</br>
                noteEditor.setBackgroundColor(Color.BLUE);</br>
                break;</br>
            case R.id.color_yellow:</br>
                noteEditor.setBackgroundColor(Color.YELLOW);</br>
                break;</br>
            case R.id.color_red:</br>
                noteEditor.setBackgroundColor(Color.RED);</br>
                break;</br>
</br>
效果截图如下：</br>
点击Change backgroundcolor</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/6.png)</br>
点击想修改的颜色，例如黄色</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/7.png)</br>
背景色改变</br>
![image](https://github.com/JiangDongshuo/NotePad1.01/raw/master/app/src/main/res/drawable/8.png)</br>
</br>
# 导出笔记
在menu加入一个item，并增加对item的监听，查出相关title和note分别作为文件名和内容，核心代码如下：</br>


       




