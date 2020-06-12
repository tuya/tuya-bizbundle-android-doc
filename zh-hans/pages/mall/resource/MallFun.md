# 商城可用性

当前用户所在区的商城业务是否可用

**接口说明**

当前用户所在区商城是否可用，此接口为异步方法

``` java
requestSupportMall(ResultListener<Boolean> listener)
```
**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| listener     | 商城可用请求异步回调 |

**示例代码**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestSupportMall(new Business.ResultListener<Boolean>() {
    @Override
    public void onFailure(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            L.e("requestSupportMall", s);
    }
    @Override
    public void onSuccess(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            //aBoolean = true 表示商城可用
            L.d("requestSupportMall", String.valueOf(aBoolean));
    }
});
```

# 商城首页链接

若商城可用时，可获取用户所在区商城首页 URL

**接口说明**

 获取用户所在区商城首页 URL，此接口为异步接口

``` java
requestHomePageUrl(IQueryMallPageUrlCallback callback)
    ```
**参数说明**

| 参数                          | 说明                            |
| ----------------------------- | ------------------------------- |
| IQueryMallPageUrlCallback | 商城首页请求异步回调 |

**示例代码**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestHomePageUrl(new IQueryMallPageUrlCallback() {
    @Override
    public void onSuccess(String domain) {
        L.d("requestHomePageUrl", domain);
    }
    @Override
    public void onError(String code, String error) {
        L.e("requestHomePageUrl", error);
    }
});
```

# 商城订单链接

若商城可用时，可获取用户所在区商城订单 URL

**接口说明**

 获取用户所在区商城订单 URL，此接口为异步接口

``` java
requestUserCenterPageUrl(IQueryMallPageUrlCallback callback)
```
**参数说明**

| 参数                 | 说明                     |
| -------------------- | ------------------------ |
| IQueryMallPageUrlCallback | 商城订单请求异步回调 |

**示例代码**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestUserCenterPageUrl(new IQueryMallPageUrlCallback() {
    @Override
    public void onSuccess(String domain) {
        L.d("requestUserCenterPageUrl", domain);
    }
    @Override
    public void onError(String code, String error) {
        L.e("requestUserCenterPageUrl", error);
    }
});
```

# 打开商城页面

商城展示页面支持 Activity 和 Fragment

**示例代码**

Activity
``` java
Intent intent = new Intent(context, WebViewActivity.class);
intent.putExtra("Uri", url);
context.startActivity(intent);
```

Fragment
``` java
WebViewFragment fragment = new WebViewFragment();
Bundle args = new Bundle();
args.putString("Uri", url);
args.putBoolean("enableLeftArea", true);
fragment.setArguments(args);
getSupportFragmentManager().beginTransaction()
        .add(R.id.web_content, fragment, WebViewFragment.class.getSimpleName())
        .commit();
```