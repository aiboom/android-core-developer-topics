
## App想要联网，需要有什么权限？

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

## 简要说明如何设计一个安全的网络连接？

分为三点：

- 尽量少的使用网络来传输私有用户数据。
- 尽量通过SSL来实现网络传输。
- 考虑使用CA证书。

## 为什么要在独立的线程处理网络连接？

为了避免UI线程被阻塞。为此Android系统将检测网路操作是否发生在UI线程，如果是，则会抛出`NetworkOnMainThreadException`异常。
