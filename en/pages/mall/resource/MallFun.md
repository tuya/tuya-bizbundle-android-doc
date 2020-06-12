# Mall service availability

Availability of mall service in the current user's area

**Declaration**

Availability of mall service in the current user's area,This method is an asynchronous method.

``` java
requestSupportMall(ResultListener<Boolean> listener)
```
**Parameter**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| listener     |  mall service  availability callbacks |

**Example**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestSupportMall(new Business.ResultListener<Boolean>() {
    @Override
    public void onFailure(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            L.e("requestSupportMall", s);
    }
    @Override
    public void onSuccess(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            //aBoolean = true mall service is availabe
            L.d("requestSupportMall", String.valueOf(aBoolean));
    }
});
```


# Mall home url

If the mall service is available, you can get the URL of the home page of the mall in user's area

**Declaration**

Get the URL of the home page of the mall in user's area,This method is an asynchronous method.

``` java
requestHomePageUrl(IQueryMallPageUrlCallback callback)
    ```
**Parameter**

| Parameter                          | Description                            |
| ----------------------------- | ------------------------------- |
| IQueryMallPageUrlCallback | mall home url callbacks |

**Example**
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


# Mall order url

If the mall service is available, you can get the URL of the order of the mall in user's area

**Declaration**

get the URL of the order of the mall in user's area,This method is an asynchronous method.

``` java
requestUserCenterPageUrl(IQueryMallPageUrlCallback callback)
```
**Parameter**

| Parameter                 | Description                     |
| -------------------- | ------------------------ |
| IQueryMallPageUrlCallback | mall order url callbacks |

**Example**
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

# Open mall page

Activity and Fragment support on the Mall display page

**Example**

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