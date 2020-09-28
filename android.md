# Android

## Activity

// 整个应用程序门面，负责应用程序中数据展示，是各种各样控件的容器及用户和应用程序之间交互的接口 ；

### 状态

#### running

当前显示在屏幕的activity(位于任务栈的顶部)，用户可见状态；

#### paused

依旧在用户可见状态，但是界面焦点已经失去，此Activity无法与用户进行交互；

#### stopped

用户看不到当前界面,也无法与用户进行交互 完全被覆盖；

#### killed

当前界面被销毁，等待这系统被回收；

### 生命周期

![](./images\activity.png)

### 回调方法

#### protected void onCreate(Bundle savedInstanceState)

该方法是在Activity被创建时回调，它是生命周期第一个调用的方法；

可用于初始化操作；

#### protected void onStart()

表示Activity正在启动，已处于可见状态，只是还没有在前台显示；

可用于界面刷新工作，比如设置控件可见性、开启动画等；

Activity每次从后台进入前台，均会执行onStart()方法；

#### protected void onResume()

表示Activity已在前台可见，可与用户交互；

可用于收集页面打开率，用户活跃度等数据；

重新初始化在onPause或者onStop方法中释放的资源；

#### protected void onPause()

表示Activity正在停止，界面仍然可见但已失去焦点；

可用于里停止动画，保存数据等操作；

#### protected void onStop()

表示Activity已进入后台，界面不可见；

可用于取消网络连接，注销广播接收器等操作；

#### protected void onRestart()

再次进入到当前activity的时候调用；

调用顺序：onPause()->onStop()->onRestart()->onStart()-   >onResume()；

#### protected void onDestroy()

表示Activity即将被销毁，界面不可见；

可用于数据销毁，资源回收等操作；

```Java
// 创建选项菜单
public boolean onCreateOptionsMenu(Menu menu)
// 选项菜单点击事件
public boolean onOptionsItemSelected(@NonNull MenuItem item)
// 返回键事件
public void onBackPressed()
// 接受返回数据
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data)
// 保存Activity临时数据
protected void onSaveInstanceState (@NonNull Bundle outState)
// 获取标题栏
ActionBar actionBar = getSupportActionBar();
```

### 启动模式

#### standard

![](./images\standard.png)

#### singleTop

![](./images\singleTop.png)

#### singleTask

![](./images\singleTask.png)

#### singleInstance

![](./images\singleInstance.png)

## Adapter

### SimpleAdapter

```java
List<Map<String,Object>> mapList = new ArrayList<>;
for (int i =0;i < ?;i++){
    Map<String,Object> map = new HashMap<>;
    // TODO
    mapList.add(map);
}
SimpleAdapter simpleAdapter = new SimpleAdapter(this,mapList,android.R.layout.?,new String[]{},new int[]{});

```

### ArrayAdapter

```java
List<String> string = new ArrayList<>;
// TODO
ArrayAdapter arrayAdapter = new ArrayAdapter(this,android.R.layout.simple_list_item_1,strings);
```

### BaseAdapter

```java
public class MyAdapter extends BaseAdapter implements View.OnClickListener{

    private Context context;
    private List<Object> list;
    private Callback callback;
    private LayoutInflater layoutInflater;

    public MyAdapter(Context context,List<Object> list,Callback callback){
        this.context = context;
        this.list = list;
        this.callback = callback;
    }

    @Override
    public int getCount(){return list.size();}

    @Override
    public Object getItem(int i){return list.get(i);}

    @Override
    public Long getItemid(int i){return i;}

    @Override
    public View getView(int i,View view,ViewGroup viewGroup){
        Object object = list.get(i);
        ViewHolder viewHolder = null;
        if(view == null){
            view = layoutInflater.from(context).inflate(R.layout.xx,null);
            viewHolder = new ViewHolder;
            // 通过id寻找控件
            viewHolder.textView = findViewById(R.id.textView);
            viewHolder.imageView = findViewById(R.id.imageView);
            viewHolder.button = findViewById(R.id.Button);
            // 为button设置标记
            viewHolder.button.setTag(R.id.button,i);
            // 为view设置标记
            view.setTag(viewHolder);
        }else{
            viewHolder = (ViewHolder) view.getTag();
        }
        view.textView.setText(object.getText());
        view.imageView.setImageResource(object.getImage());
        // 为button设置点击事件监听器
        view.button.setOnClickListener(this);
    }

    static class ViewHolder{
        private TextView textView;
        private ImageView imageView;
        private Button button;
    }

    public interface Callback{
        void click(View view);
    }

    @Override
    public void onClick(View view){
        callback.click(view);
    }
}
```

### ExpandableListAdapter

```java
public class MyAdapter extends BaseExpandableListAdapter{

    private Context context;
    private LayoutInflater layoutInflater;
    private List<Group> groups;
    private List<List<Item>> items;

    public MyAdapter(Context context,List<Group> groups,List<List<Item>> items){
        this.context = context;
        this.groups = groups;
        this.items = items;
    }

    @Override
    public int getGroupCount(){return groups.size();}

    @Override
    public int getChildrenCount(int i){return items.get(i).size();}

    @Override
    public Object getGroup(int i){return groups.get(i);}

    @Override
    public Object getChild(int i,int i1){return items.get(i).get(i1);}

    @Override
    public long getGroupId(int i){return i;}

    @Override
    public long getChildId()(int i,int i1){return i1};

    @Override
    public boolean hasStableIds(){return false;}

    @Override
    public boolean isChildSelectable(int i,int i1){return false;}

    @Override
    public View getGroupView(int i,bollean b,View view,ViewGroup viewGroup){
        ViewHolderGroup viewHolderGroup = null;
        if(view = null){
            view = layoutInflater.from(context).inflate(R.layout.group,null);
            // TODO
            view.setTag(viewHolderGroup);
        }else{
            viewHolderGroup = (ViewHolderGroup) view.getTag();
        }
        // TODO
        return view;
    }

    @Override
    public View getChildView(int i,int i1,boolean b,View view,ViewGroup viewGroup){
        ViewHolderItem viewHolderItem = null;
        Item item = items.get(i).get(i1);
        if(view =null){
            view = layoutInflater.from(context).inflate(R.layout.item,null);
            viewHolderItem = new ViewHolderItem();
            // TODO
            view.setTag(viewHOlderItem);
        }else{
            viewHolderItem = (ViewHolderItem) view.getTag();
        }
        // TODO
        return view;
    }

    static class ViewHolerGroup{
        // TODO
    }

    static class ViewHolderItem{
        // TODO
    }

}
```

### PagerAdapter

```java
public class MyAdapter extend PagerAdapter{
    private List<View> views;
    privaet Context context;

    public MyAdapter(Context context,List<View> views){
        this.context = context;
        this.views = views;
    }

    @Override
    public voolean isViewFromObject(@NonNull View view,@NonNull Oject object){
        return view == object;
    }

    @Override
    public int getCount(){
        return views.size();
    }

    @NonNull
    @Override
    public Oject instantiateItem(@NonNull ViewGroup container,int position){
        View view = views.get(position);
        container.addView(view);
        return view;
    }

    @Override
    public void destoryItem(@NonNull ViewGroup container,int position,@NonNull Object object){
        View view = views.get(position);
        container.removeView(view);
    }

}
```

### FragmentPagerAdapter

```java
public class MyAdapter extends FragmentPagerAdapter{

    private List<Fragment> fragments;

    public MyAdapter(FragmentManager fm ,List<Fragment> fragments){
        super(fm);
        this.fragment = fragment;
    }

    @Override
    public Fragment getItem(int position){return fragment.get(position);}

    @Override
    public int Count(){return fragments.size();}

}
```

## Broadcast Receiver

// 实现消息的异步接收，类似事件编程中的监听器，普通的事件监听器监听的是程序中的控件，而BroadcastReceiver监听的事件源是Android应用中其他的组件 ；

## Calendar

### SimpleDateFormat

```java
// 设置格式化格式为:年年年年-月月-天天 小时-分钟-秒
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss");
// 设置格式化格式为:星期
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("EEEE");
// 格式化当前时间
simpleDateFormat.format(new Date());
// 将String类型的时间格式化为Date类型
simpleDataFormat.parse("????-??-?? ??-??-??");
```

### Calendar

```java
Calendar calendar = Calendar.getInstance();
// 获取昨天的日期
calendar.set(Calendar.DATE,calendar.get(Calendar.DATE) - 1);
// 获取明天的日期
calendar.set(Calendar.DATE,calendar.get(Calendar.DATE) + 1);
```

## Content Provider

// 为不同的应用程序之间数据访问提供统一的访问接口，通常它与ContentResolver结合使用，一个是应用程序使用ContentProvider来暴露自己的数据，而另外一个是应用程序通过ContentResolver来访问数据；

## DataBase

### SharePreferences

```java
// 获取SharePreferences
SharedPreferences sharedPreferences = getSharedPreferences("data", MODE_PRIVATE);
// 获取SharedPreferences.Editor
SharedPreferebces.Editor editor = getSharedPreferences(String,MODE_PRIVATE).edit();
// 存放数据
editor.putString(key,value);
// 保存数据
editor.apply();
```

### SQLite

```java
// 定义创建数据表语句
public static final String CREATE_TABLE="create table tablename (value)";
// 继承SQLiteOpenHelper类
public class MysqLiteOpenHelper extends SQLiteOpenHelper
// 构造器
public MysqLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory,int version)          {super(context,name,factory,version);}
// 重写OnCreate方法
@Override
public void onCreate(SQLiteDatabase db){}
// 重写onUpgrade方法
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {}
// 数据库执行语句
databasesName.execSQL();
// 创建一个对象
MysqLiteOpenHelper mysqLiteOpenHelper = new MysqLiteOpenHelper(context, "databasesName.db", null, version);
// 打开或者创建一个数据库并放回一个SQLiteDatabase类的对象
SQLiteDatabase sqLiteDatabase = mysqLiteOpenHelper.getReadableDatabase();
// 定义一个ContenValue
ContentValues contentValues = new ContentValues();
// contenvalue的重载方法存放值
contentValues.put(Key，Value);
// 清除contentvalue数据
contentValue.clear();
// 向数据表中添加数据
databasesName.insert(tableName,null,ContentValues);

```

### Litepal

```gradle
// 导入第三方库
implementation 'org.litepal.android:java:3.0.0'
//  修改Application配置
android:name="org.litepal.LitePalApplication"
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<litepal>
<dbname value="data"></dbname>
    <version value="1"></version>
    <list>
        <mapping class="packageName.className"></mapping>
    </list>
</litepal>
```

```java
// 创建/得到数据库
Litepal.getDatabase();
// 添加数据
Class class = new Class();
class.set();
// 保存数据
class.save()
class.updateAll("key=? and author = ?","value","value");
// 删除数据
LitePal.deleteAll(Class.class,"conditions","value")
// 查询数据
List<Class> classes = LitePal.findAll(Class.class);
// 寻找第一条数据
Class firstClass = LitePal.findFirst(Class.class);
// 寻找最后一条数据
Class lastClass = LitePal.findLast(Class.class);
// select查找
List<Class> classes = LitePal.select("key").find(Class.class);
// where查找
List<Class> classes = LitePal.where("key >?","value").find(Class.class);
// order排列
List<Class> classes = LitePal.order("key asc/desc").find(Class.class);
// limit查询
List<Class> classes = LitePal.limit(int).find(Class.class);
// offset查询  // 查询int int-1 int+1数据
List<Class> classes = LitePal.limit(int).offset(1).find(Class.class);
// 原生查询
// TODO
```

## Dialog

### Dialog

```java
Dialog dialog = new Dialog(context);
// 添加布局
dialog.setContentView(R.layout.car_zhcz_lqy);
// 获取Dialog布局中的控件
TextView ? = dialog.getWindow().findViewById(R.id.?);
// 获取Dialog布局中的控件并设置监听器
dialog.getWindos().findViewById(R.id.?).setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v) {
        // 设置dialog摧毁
        dialog.dismiss();
    }
});
// 设置dialog显示
dialog.show();
```

### Toast

```java
Toast.makeText(this,"content"，Toast.LENGTH_SHORT).show();
```

### AlertDialog

```java
AlertDialog.Builder dialog = new AlertDialog.Builder(context);
// 设置提示框标题
dialog.setTitle("");
// 设置提示框内容
dialog.setMessage("");
// 设置是否可返回
dialog.setCancelable();
// 设置点击外部返回
progressDialog.setCanceledOnTouchOutside(true);
// 设置确定按钮
dialog.setPositiveButton("OK", new DialogInterface.OnClickListener(){});
// 设置取消按钮
dialog.setNegativeButton("No", new DialogInterface.OnClickListener(){});
// 设置提示框显示
dialog.show();
// 设置提示框取消
dialog.cancle();
```

### DatePickerDialog

```java
Calendar calendar = Calendar.getInstance();
int year = calendar.get(Calendar.getYEAR);
int month = calendar.get(Calendar.getMONTH);
int day = calendar.get(Calnedar.DAY_OF_MONTH);
DatePickerDialog datePickerDialog = new DatePickerDialog(this,new DatePickerDialog.OnDateSetListener(){
    @Override
    public void onDateSet(DatePicker datePicker,int i ,int i1,int i2){

    }
},year,month,day);
datePickerDialog.show();
```



### ProgressDialog

```java
ProgressDialog progressDialog = new ProgressDialog(this);
// 设置为水平样式
progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
// 设置为圆形样式
progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
// 设置提示框标题
progressDialog.setTitle("");
// 设置提示框内容
progressDialog.setMessage("");
// 设置提示框图标
progressDialog.seIcon();
// 设置是否可返回
progressDialog.setCancelable();
// 设置点击外部返回
progressDialog.setCanceledOnTouchOutside(true);
// 设置确定按钮
progressDialog.setPositiveButton("OK", new DialogInterface.OnClickListener(){};
// 设置取消按钮
progressDialog.setNegativeButton("No", new DialogInterface.OnClickListener(){};
// 设置提示框显示
progressDialog.show();
// 设置提示框取消
progressDialog.cancle();
```

### Notification

```java
NotificationManager manager;
Notification notification;
private static final int NOTIFYID = 1;
Notification.Builder builder = new Notification.Builder(Context);

// 定义一个PendingIntent点击Notification后启动一个Activity
Intent intent = new Intent(Context, Activity.class);
PendingIntent pendingIntent = PendingIntent.getActivity(Context, 0, intent, 0);

// 获取通知系统服务
manager=(NotificationManager)getSystemService(NOTIFICATION_SERVICE);

// 设置通知栏标题
builder.setContentTitle(String)
// 设置通知栏内容
builder.setContentText(String);
// 设置通知栏小图标
builder.setSmallIcon(ImageResource);
// 设置通知栏大图标
builder.setLargeIcon(ImageResource);
// 设置通知时间
builder.setWhen(Time);
// 设置点击后自动取消
builder.setAutoCancel(true);
// 设置PendingIntent
builder.setContentIntent(pendingIntent);
// 调用Builder的build()方法为notification赋值
notification = builder.build();
// 调用NotificationManager的notify()方法发送通知
manager.notify(NOTIFYID, notification);
```

## Fragment

### 状态

### 生命周期

### 回调方法

// Activity中有的所有回调方法Fragment都有，并且添加了以下几种回调方法;

#### public void onAttach(Activity)

当Fragment与Activity发生关联时调用

#### public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState)

创建该Fragment的视图

#### public void onActivityCreated(@Nullable Bundle savedInstanceState)

当Activity的onCreate方法返回时调用

#### public void onDestroyView()

该Fragment的视图被移除时调用

#### public void onDetach()

当Fragment与Activity关联被取消时调用

```java
public class VideoFragment extends Fragment {
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup          container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(@LayoutRes,container,false);
        return view;
    }
```

```java
// 获得FragmentManager实例
FragmentManager fragmentManager = getSupportFragmentManager();
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
// 替换Fragment
fragmentTransaction.replace(R.id.?,Fragment);
// 提交
fragmentTransaction.commit
```

## GetDrawable

```java
public void getImage()  {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    URL url = new URL("http:// 192.168.3.7:8088/transportservice"+ImgAddr);
                    Drawable deawable =Drawable.createFromStream(url.openStream(),"src");
                } catch (IOException  e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
```

## Intent

### ComponentName

### Action

### Data

### Category

### Extras

### Flags

## IsFirstLogin

```java
SharedPreferences sharedPreferences = getSharedPreferences("isFirst",MODE_PRIVATE);
// 如果没有值，就设为true
boolean isFirst = sharedPreferencs.getBoolean("First",true);
    // 是第一次登录的情况
if (isFirst){
    SharedPreferences.Editor editor = sharedPreferences.edit();
    editor.putBoolean("isFirst",false);
    editor.apply();
    // TODO
    }
    // 否则直接进入页面
    else{
    Intent intent = new Intent(this,SecondActivity.class);
    startActivity(intent);
    finish();
    }
```

## Log

```java
// 日志信息
Log.v(tag,msg)
// 调试信息
Log.d(tag,msg)
// 重要信息
Log.i(tag,msg)
// 警告信息
Log.w(tag,msg)
// 错误信息
Log.e(tag,msg)
```

## Map

### Marker

```java
// 创建一个地图
mapView.onCreate(saveInstanceState);
// 获得地图实例
AMap aMap = mapView.getMap();
List<LatLng> latLngs = new ArrayList<>;
// 经纬度
LatLng latLng1 = new LatLng(v,v1);
LatLng latLng2 = new LatLng(v,v1);
latLngs.add(latLng1);
latLngs.add(latLng2);
// 向地图中添加一个标记
aMap.addMarker(new MarkerOptions().position(latLng1));
// 移动地图视角中心点
aMap.moveCamera(CameraUpdateFactory.newLatLngZoom(latLIng1,15));
```

### PolyLine

```java
// 向地图中添加一个折线图
aMap.addPolyLine(new PolyLineOptions().addAll(latLngs).
width(2f).color(Color.argb(255,255,0,0)));
```

### AMapUtils

```java
// 计算两点之间距离
AMapUtils.calculateLineDistance(latLng1,latLng2);
// 计算两点之间的面积
AmapUtils.calculateArea(leftTopLatLng,rightBottomLatLng);
```

### Type

```java
// 设置为卫星图层
aMap.setMapType(AMap.MAP_TYPE_SATELLITE);
// 设置为导航图层
aMap.setMapType(AMap.MAP_TYPE_NAVI);
// 设置为夜景图层
aMap.setMapType(AMap.MAP_TYPE_NIGHT);
// 设置为白昼图层
aMap.setMapType(AMap.MAP_TYPE_NORMAL);
// 设置为实时路况图层
aMap.setTrafficEnabled(true);
```



## Method

```java
// 获得当前的应用方向
getRequestedOrientation();
// 设置当前的应用方向为横向
setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
// 设置当前的应用方向为竖直
setRequestedOrientation(ActivityInfo.SCREEN_ORIENTAION_PORTRAIT);

// 获取当前Activity的标题栏
ActionBar actionBar = getSupportActionBar();
// 判断当前Activity标题栏是否显示
actionBar.isShowing();
// 隐藏当前Activity的标题栏
actionBar.hide();
// 设置EditText不弹出键盘
editText.setInputType(InputType.TYPE_NULL);

// 获取输入方法管理类
InputMethodManager inputMethodManager =(InputMethodManager)getSystemService(INPUT_METHOD_SERVICE);
// 点击后隐藏软键盘
inputMethodManager.hideSoftInputFromWindow(view.getWindowToken(),0);
```

## MPAndroidChart

### LineChart

```java
public void getLineChart() {
    List<Entry> entries1 = new ArrayList<>();
    List<Entry> entries2 = new ArrayList<>();
    LineDataSet dataSet1,dataSet2;
    LineData lineData;
    int i = 0;

    // 添加数据
    for (TestData testData : testDatas) {
        entries1.add(new Entry(i,testData.getUp()));
        entries2.add(new Entry(i,testData.getDown()));
        i++;
    }

    dataSet1 = new LineDataSet(entries1,"Label");
    dataSet2 = new LineDataSet(entries2,"Label");
    lineData = new LineData();
    lineData.addDataSet(dataSet1);
    lineData.addDataSet(dataSet2);
    lineChart.setData(lineData);
    // 设置是否可以拖拽
    lineChart.setDragEnabled(true);
    // 设置是否可以缩放
    lineChart.setScaleEnabled(true);
    // 设置左边Y轴是否显示
    lineChart.getAxisLeft().setEnabled(false);
    // 设置右边Y轴是否显示
    lineChart.getAxisRight().setEnabled(false);
    // 设置X轴在下方显示
    lineChart.getXAxis().setPosition(XAxis.XAxisPosition.BOTTOM);
    // 初始化
    lineChart.invalidate();
}
```



### BarChart

```java
public void getBarChart(){
    List<BarEntry> entries = new ArrayList<>();
    BarDataSet dataSet;
    BarData barData;
    int color = ColorTemplate.getHoloBlue();
    int i = 0;

    // 添加数据
    for (TestData testData : testDatas){
        entries.add(new BarEntry(i,testData.getUp()));
        i++;
    }

    dataSet = new BarDataSet(entries,"label");
    // 设置颜色
    dataSet.setColor(color);
    barData = new BarData(dataSet);
    barChart.setData(barData);
    barChart.invalidate();
}
```

### PieChart

```java
public void getPieChart(){
    List<PieEntry> entries = new ArrayList<>();
    List<Integer> colors = new ArrayList<>();
    PieDataSet pieDataSet;
    PieData pieData;
    int i = 0;
    // 添加数据
    for (TestData testData:testDatas){
        entries.add(new PieEntry(testData.getUp(),"第"+i+"条数据"));
        i++;
    }
    // 添加数据所对应的颜色
    for (int c : ColorTemplate.JOYFUL_COLORS)
        colors.add(c);
    for (int c : ColorTemplate.COLORFUL_COLORS)
        colors.add(c);
    for (int c : ColorTemplate.VORDIPLOM_COLORS)
        colors.add(c);

    // 设置数据
    pieDataSet = new PieDataSet(entries,"label");
    // 设置颜色
    pieDataSet.setColors(colors);
    // 隐藏值
    pieDataSet.setDrawValues(false);
    pieData = new PieData();
    pieData.setDataSet(pieDataSet);
    // 设置是否绘制饼状图中间部分
    pieChart.setDrawHoleEnabled(true);
    // 设置饼状图中间部分文字内容
    pieChart.setCenterText("This is test Chart");
    // 设置是否使用百分比
    pieChart.setUsePercentValues(true);
    // 禁用Legend表绘制
    pieChart.getLegend().setEnabled(false);
    // 设置Description Label的值
    pieChart.getDescription().setText("");
    // 设置是否可以手动旋转
    pieChart.setRotationEnabled(eChar.setRotationEnabled(true);
    // PieChart指定数据
    pieChart.setData(pieData);
    pieChart.invalidate();
}
```

## OKHTTP

```gradle
implementation 'com.google.code.gson:gson:2.8.5'
implementation 'com.squareup.okhttp3:okhttp:3.12.1'
```

```xml
<!-- 开通网络权限 -->
<uses-permission android:name="android.permission.INTERNET"/>
```

### HttpCallbackListener.java

```java
// 防止线程阻塞（回调机制）接口
public interface HttpCallbackListener {
    void onFinish(String response);
    void onError(Exception e);
}
```

### HttpUtil.java

```java
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import okhttp3.MediaType;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;

public class HttpUtil {
    public static void sendHttpRequest(final String address, final HttpCallbackListener listener) {
        //  开启子线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                HttpURLConnection connection = null;
                try {
                    URL url = new URL(address);
                    connection = (HttpURLConnection) url.openConnection();
                    connection.setRequestMethod("POST");// 设置请求方式
                    connection.setConnectTimeout(8000);// 最长请求时间
                    connection.setReadTimeout(8000);// 最长的读取时间
                    connection.setDoInput(true);
                    connection.setDoOutput(true);

                    InputStream in = connection.getInputStream();
                    BufferedReader reader = new BufferedReader(new                                          InputStreamReader(in));
                    StringBuilder response = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null) {
                        response.append(line);
                    }

                    if (listener != null) {
                        listener.onFinish(response.toString());
                    }

                } catch (Exception e) {
                    if (listener != null) {
                        listener.onError(e);
                    }
                } finally {
                    if (connection != null) {
                        connection.disconnect();
                    }
                }
            }
        }).start();
    }

    public static void sendOkHttpRequest(String address, okhttp3.Callback callback) {
        OkHttpClient client = new OkHttpClient();
        // 将String类型参数转换为Json格式
        MediaType JSON = MediaType.parse("application/json; charset=utf-8");
        RequestBody body = RequestBody.create(JSON, "{\"BusStationId\":1}");
         // 请求地址 // 请求参数
        Request request = new Request.Builder().url(address).post(body).build();
        client.newCall(request).enqueue(callback);
    }

    public static void sendOkHttpRequest(String address, String params, okhttp3.Callback callback) {
        OkHttpClient client = new OkHttpClient();
        // 将String类型参数转换为Json格式
        MediaType JSON = MediaType.parse("application/json; charset=utf-8");
        RequestBody body = RequestBody.create(JSON, params)
        // 请求地址 // 请求参数
        Request request = new Request.Builder().url(address).post(body).build();
        client.newCall(request).enqueue(callback);
    }
}
```

```java
public void sentRequest(){
    // 请求地址
    String url = "";
    // 请求参数
    String params = "";
    HttpUtil.sendOkHttpRequest(url, new Callback() {

    @Override
    public void onFailure(Call call, IOException e) {
        // 接收失败
        Log.d(TAG,"onFailure:")

    }

    @Override
    public void onResponse(Call call, Response response) throws IOException {
        // 接收成功
        String responseData = response.body().string();
        Gson gson = new Gson();
        // 参数一：接收到json数据，参数二：对应的类映射
        Class class = gson.fromJson(responseData,class.class);
    }
    },params);
}
```

## Picker

## QRCode

```gradle
implementation 'com.google.zxing:core:3.3.3'
```

```java
public Bitmap getQRCode(){
    int width = 200;
    int height = 200;
    Bitmap QRCode = null;
    BitMatrix bitMatrix = null;
    int[] pixels;
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss");
    QRCodeWriter writer = new QRCodeWriter();

    Hashtable<EncodeHintType, String> hashTable = new Hashtable<>();
    hashTable.put(EncodeHintType.CHARACTER_SET, "UTF-8");
    try {
        bitMatrix = writer.encode("This is test QRCode" +simpleDateFormat.format(new Date()), BarcodeFormat.QR_CODE, width, height, hashTable);
    } catch (WriterException e) {
        e.printStackTrace();
    }
    pixels = new int[width * height];
    for (int y = 0; y < height; y++) {
        for (int x = 0; x < width; x++) {
            if (bitMatrix.get(x, y)) {
                pixels[y * width + x] = 0xff000000;
            } else {
                pixels[y * width + x] = 0xffffffff;
            }
        }
    }
    QRCode = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
    QRCode.setPixels(pixels, 0, width, 0, 0, width, height);
    return QRCode;
}
```

## Service

// 前台不可见，承担大部分数据处理工作，通常为其他的组件提供后台服务或监控其他组件的运行状态 ；

## VideoView

```java
// 实例化媒体控制器
MediaController mediaController = new MediaController(this);
// 为videoView设置媒体控制器
videoView.setMediaController(mediaController);
// 设置视频Uri路径
videoView.setVideoURI(Uri.parse("android.resource://"+getPackageName()+"/"+R.raw.xx));
videoView.setVideoURI(Uri.parse("android.resource://"+getPackageName()+"/"+getResource().getIdentifier(xx,"raw",getPackageName())));
// 开始播放
videoView.start();
```

## WebView

```java
// 加载一个网页
webView.loadUrl("http://www.google.com");
// 加载apk包中的html页面
webView.loadUrl("file:///android_asset/test.html");
// 加载手机本地的html页面
webView.loadUrl("content://com.android.htmlfileprovider/sdcard/test.html");
// 加载 HTML 页面的一小段内容
WebView.loadData(String data, String mimeType, String encoding);
// 启用javascript接口
webView.getSettings().setJavaScriptEnabled(true);
// 添加一个javascripter接口
webView.addJavascriptInterface(new TestActivity.JavaScriptInterface(this), "nativeMethod");
// javascript接口定义
public class JavaScriptInterface{

    JavaScriptInterface(Activity activity){this.activity = activity};

    @JavaScriptInterface
    public void method(){
        // TODO
    }
}
//
```



## ZoomImage

```java
public class ZoomImage extends AppCompatImageView {
    private PointF canterPoint;
    private Matrix matrix;
    private float firstDistance;

    public ZoomImage(Context context) {
        super(context);
        initParams();
    }

    public ZoomImage(Context context, AttributeSet attrs) {
        super(context, attrs);
        initParams();
    }

    public ZoomImage(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initParams();
    }

    private void initParams() {
        matrix = new Matrix();
        setImageMatrix(matrix);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        int count = event.getPointerCount();
        if (count == 2) {
            switch (event.getAction() & MotionEvent.ACTION_MASK) {
                case MotionEvent.ACTION_POINTER_DOWN:
                    firstDistance = getDistace(event);
                    canterPoint = getCanterPoint(event);
                    break;
                case MotionEvent.ACTION_MOVE:
                    float distance = getDistace(event);
                    float scrale = distance / firstDistance;
                    matrix.postScale(scrale, scrale, canterPoint.x, canterPoint.y);
                    setImageMatrix(matrix);
                    firstDistance = distance;
                    break;
                default:
                    break;
            }
        }
        return super.onTouchEvent(event);
    }

    private PointF getCanterPoint(MotionEvent event) {
        PointF pointF = new PointF();
        pointF.x = (event.getX() + event.getX(1)) / 2;
        pointF.y = (event.getY() + event.getY(1)) / 2;
        return pointF;
    }

    private float getDistace(MotionEvent event) {
        float x1 = event.getX();
        float x2 = event.getX(1);
        float y1 = event.getY();
        float y2 = event.getY(1);
        float distance = (float) Math.sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) *
        (y1 - y2));
        return distance;
    }
}
```

# XML

## Switch

```xml
// 设置默认为开
android:checked="true"
// 设置开关最小宽度
android:switchMinWidth=""
// 设置开启时文本内容
android:textOn=""
// 设置关闭时文本内容
androidLtextOff=""
// 设置按钮图片
android:thumb=""
```

## Shape

```xml
// 定义形状
<shape
       xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape=["rectangle" | "oval" | "line" | "ring"]>
// 圆角属性
    <corners
        android:radius="integer"
        android:topLeftRadius="integer"
        android:topRightRadius="integer"
        android:bottomLeftRadius="integer"
        android:bottomRightRadius="integer"/>
// 渐变属性
    <gradient
        android:angle="integer"
        android:centerX="integer"
        android:centerY="integer"
        android:centerColor="integer"
        android:endColor="color"
        android:gradientRadius="integer"
        android:startColor="color"
        android:type=["linear" | "radial" | "sweep"]
        android:useLevel=["true" | "false"] />
// 边距属性
    <padding
        android:left="integer"
        android:top="integer"
        android:right="integer"
        android:bottom="integer" />
// 大小属性
    <size
        android:width="integer"
        android:height="integer" />
// 填充属性
    <solid
        android:color="color" />
// 描边属性
    <stroke
        android:width="integer"
        android:color="color"
        android:dashWidth="integer"
        android:dashGap="integer" />
</shape>
```

## Menu

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http:// schemas.android.com/apk/res/android"
      xmlns:app="http:// schemas.android.com/apk/res-auto">
    <item
          android:id="@id/?"
          android:orderIncategory="100"
          android:title="?"
          // 将此项title设为菜单入口
          app:showAsAction="always"/>
</menu>
```

## Style

```xml
<style name="SplashTheme" parent="AppTheme.NoActionBar">
    <item name="android:windowBackgroud">@drawable/?</item>
</style>
```

# Gradle

// 在AndroidManifest.xml中给<activity>标签指定android:launchMode属性;

```gradle
// 设置为App启动页面
<intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.LAUNCHER"/>
</intent-filter>
```

