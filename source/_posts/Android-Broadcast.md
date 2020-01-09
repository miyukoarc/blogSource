---
title: Android-Broadcast
date: 2020-01-09 15:37:05
tags: Android
---

# Android-Broadcast


## 使用广播接收器接收广播(BroadcastReceiver)


### 1.定义一个广播类

```
public class MyReceiver extends BroadcastReceiver {

    @SuppressLint("UnsafeProtectedBroadcastReceiver")
    @Override
    public void onReceive(Context context, Intent intent){
        Toast.makeText(context,"收到广播",Toast.LENGTH_SHORT).show();
        //BroadReceiver中不允许开启多线程,过久操作会报错
        //可以启动服务,发送通知等
    }
}

```

### 2.对广播进行注册
静态注册(AndroidManiFest.xml)
动态注册

```
receiver = new MyReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction("com.example.my");
registerReceiver(receiver, intentFilter);
```


## 使用广播发送者发送自定义广播
声明意图
```
Intent inten = new Intent("com.example.my");
sendBroadcast(intent);//发送标准广播
sendOrderedBroadcast(intent, null);//发送有序广播
```