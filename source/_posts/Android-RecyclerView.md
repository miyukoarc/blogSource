---
title: Android-RecyclerView
date: 2020-01-10 15:01:21
tags: Android
---

# RecyclerView实现列表渲染

 ## 创建数据处理类
 表格中的一项需要渲染什么数据都在其中做出规划
```
public class ListData {
	private nameText；
	private ageText；
	public ListData(String nameText, String ageText) {
		this.nameText = nameText;
		this.ageText = ageText;
	}
	
	public String getNameText (){
		return nameText;
	}
	
	public String getAgeText (){
		return ageText;
	}
}
```

 ## 创建布局
 ### main
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <EditText
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="60dp"
            android:id="@+id/edit_text" />
        <Button
            android:layout_width="100dp"
            android:layout_gravity="end"
            android:layout_height="60dp"
            android:text="button"
            android:id="@+id/res"/>
    </LinearLayout>

    <androidx.recyclerview.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/recycler_view"/>

</LinearLayout>
 
```

 ### single_line
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:paddingVertical="10dp"
    android:paddingHorizontal="10dp"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20dp"
        android:text="名字"
        android:id="@+id/name"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_weight="1"
        android:gravity="right"
        android:layout_height="wrap_content"
        android:textSize="20dp"
        android:text="年龄"
        android:id="@+id/age"/>

</LinearLayout>
```
 

 ## 创建适配器
```
public class ListAdapter extends 			ReceiverView.Adapter<ListAdapter.ViewHold>{

	private List<ListData> mListData;//表格数据表
	
	static class ViewHolder extends RecyclerView.ViewHolder {
	
		TextView nameText;
		TextView ageText;
		
		public ViewHolder(View view){
			super(view);
			nameText = view.findViewById(R.id.name);
			ageText = view.findViewById(R.id.age);
		}
	}
	
	public ListAdapter(List<ListData> info){
		mListData = info;
	}
	
	@NonNull
	@Overview
	public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent,
	int viewType) {
		View view = LayoutInflater
        	.from(parent.getContext())
            .inflate(R.Layout.single_line,false);
        return new ViewHolder(view);
	}
	
	@Overview
	public void onBindViewHolder(@NonNull ViewHolder holder,
	int position) {
		ListData listData = mListData.get(position);
		holder.nameText.setText(listData.getNameText());
		holder.ageText.setText(listData.getAgeText());	
	}
	
	@Overview
	public int getItem() {
		return mListData.size();
	}
}
```

 ## 挂载
```
public class MainActivity extends AppCompatActivity {

    private List<ListData> mlist = new ArrayList<>();

    private Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btn = findViewById(R.id.res);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                initData();
                initRecyclerView();
            }
        });


    }

    public void initRecyclerView(){
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        recyclerView.setLayoutManager(new LinearLayoutManager(MainActivity.this));
        recyclerView.setAdapter(new ListAdapter(mlist));
    }

    public void initData(){
        mlist.add(new ListData("Jack","19"));
        mlist.add(new ListData("White","19"));
        mlist.add(new ListData("Rose","17"));
        mlist.add(new ListData("Valentine","20"));
        mlist.add(new ListData("Isabelle","20"));
        mlist.add(new ListData("Benjamin","18"));
        mlist.add(new ListData("Mike","21"));
        mlist.add(new ListData("Zoe","17"));
        mlist.add(new ListData("Jack","19"));
        mlist.add(new ListData("White","19"));
        mlist.add(new ListData("Rose","17"));
        mlist.add(new ListData("Valentine","20"));
        mlist.add(new ListData("Isabelle","20"));
        mlist.add(new ListData("Benjamin","18"));
        mlist.add(new ListData("Mike","21"));
        mlist.add(new ListData("Zoe","17"));
    }


}
```
 