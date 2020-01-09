---
title: Android Service
date: 2020-01-09 11:50:23
tags: "Android"
---

#Android Service
~~这几天感冒了,状态不是很好啊~~
1.创建Service的子类MyService
```
public class MyService extends Service {

    @Override
    public IBinder onBind(Intent intent){
        return null;
    }
    

    @Override
    public int onStartCommand(Intent intent,int flags,int startId){
        Toast.makeText(this,"服务已启动",Toast.LENGTH_SHORT).show();
        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Toast.makeText(this, "服务已经停止", Toast.LENGTH_LONG).show();
    }

}
```
2.在manifest中声明服务
```
<service android:name="MyService"
            android:enabled="true"
            android:exported="true">

</service>
```
3.MainActivity绑定按钮声明意图,使用在Service之外调用startService/stopServices方法启动/停用Service\
```
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    Button bStart;
    Button bStop;


    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bStart = findViewById(R.id.start);
        bStop = findViewById(R.id.stop);

        bStop.setOnClickListener(this);
        bStart.setOnClickListener(this);

    }

    @Override
    public void onClick(View view){
        switch (view.getId()){
            case R.id.start:
                Intent start = new Intent(this,MyService.class);
                startService(start);
                break;
            case R.id.stop:
                Intent stop = new Intent(this,MyService.class);
                stopService(stop);
                break;
        }
    }
}
```