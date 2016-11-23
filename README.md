NIK SDK客户端接入帮助文档
=============================
    1.JAVA游戏客户端接入

       1.11 概述
       1.22 接入准备
            1.2.1 2.1 工程导入
            1.2.2 2.2 配置AndroidManifest.xml
       1.3 3 接口接入   
            1.3.1 3.1 初始化接口
            1.3.2 3.2 生命周期接口
            1.3.3 3.3 登录接口
            1.3.4 3.4 支付接口
            1.3.5 3.5 退出接口
            1.3.6 3.6 登出接口
            1.3.7 3.7 用户中心接口
            1.3.8 3.8 登录成功后角色信息上传接口
            1.3.9 3.9 角色升级后角色信息上传接口
            1.3.10 3.10 微信登陆类型设置
            1.3.11 3.11 切换账号接口
      1.44 其他接口
            1.4.1 4.1获取渠道版本号
            1.4.2 4.2获取渠道ID
            1.4.3 4.3渠道是否有用户中心功能
            1.4.4 4.4渠道是否有退出界面功能
      1.5 5 回调接口 NKListener
            1.5.1 5.1 onInit
            1.5.2 5.2 onLogin
            1.5.3 5.3 onLogout
            1.5.4 5.4 onPay
            1.5.5 5.5 onQuit

JAVA游戏客户端接入
========================
文档版本说明：
-------------------------
去除支付版：去除了所有有关支付的接口，jar文件，接入时，mainfast中有关爱贝的acitvity和 permission都可去掉。

支付版：添加爱贝支付，按文档接入即可。

注意SDK的libs目录下有armeabi和armeabi-v7a两个cpu架构，如果游戏方只生成其中一种，请删除掉另外一种cpu架构的文件夹，否则会造成游戏在部分手机上闪退
1 概述
----------------------
此文档为Android原生游戏接入文档
2 接入准备
----------------
###2.1 工程导入
将NKDemo项目中的assets目录下的文件复制到游戏项目的assets目录
将NKSDK项目中的libs目录下的文件复制到游戏项目的libs目录 （也可以直接导入NKSDK项目，然后配置游戏项目依赖NKSDK项目即可）
修改assets目录下的config.properties文件里的IAppPay_AppID（爱贝支付）和Wechat_APPID（微信）
###2.2 配置AndroidManifest.xml
在<application>标签中添加权限声明：
```
   <uses-permission android:name="android.permission.INTERNET" />
   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
   <uses-permission android:name="android.permission.SET_DEBUG_APP" />
   <uses-permission android:name="android.permission.READ_PHONE_STATE" />
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
   <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
   <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
   <uses-permission android:name="android.permission.SEND_SMS" />
   <uses-permission android:name="android.permission.VIBRATE" />
   <uses-permission android:name="android.permission.CALL_PHONE" />
   <uses-permission android:name="android.permission.GET_PACKAGE_SIZE" />
   <uses-permission android:name="android.permission.WRITE_SETTINGS" />
   <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
   <uses-feature android:name="android.hardware.camera" />
   <uses-permission android:name="android.permission.CAMERA" />
   <uses-feature android:name="android.hardware.camera.autofocus" />
   <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
   <uses-permission android:name="android.permission.RECORD_VIDEO" />
   ```
在Application节点下增加以下Activity:
```
       <activity
               android:name="com.nkgame.SDKActivity"
               android:windowSoftInputMode="adjustPan|stateHidden"
               android:configChanges="orientation|screenSize|keyboardHidden"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               >
       </activity>
       <activity
               android:name="com.iapppay.account.channel.ipay.ui.LoginActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.account.channel.ipay.ui.RegistActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.account.channel.ipay.ui.RegSetPwdActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.account.channel.ipay.ui.WebActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleInstance" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.AccountCheckPasswordActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar" />
       <activity
               android:name="com.iapppay.pay.channel.weixinpay.WeixinWapPayActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent" />
       <activity
               android:name="com.iapppay.ui.activity.AccountModifyPasswordActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"  />
       <activity
               android:name="com.iapppay.ui.activity.AccountSmallAmountPasswordActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"  />
       <activity
               android:name="com.iapppay.ui.activity.AccountSmallAmountValueActivty"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"  />
       <activity
               android:name="com.iapppay.ui.activity.ServiceCenterActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar" />
       <activity
               android:name="com.iapppay.ui.activity.AccountBindActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar" />
       <activity
               android:name="com.iapppay.fastpay.ui.InputBankCarNoActivity"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.fastpay.ui.InputBankCarMoreInfoActivity"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.fastpay.ui.VerificationCodeActivity"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity android:name="com.iapppay.fastpay.ui.CommonWebActivity"
                 android:theme="@android:style/Theme.Translucent.NoTitleBar" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.normalpay.PayHubActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.minipay.MiniPayHubActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:launchMode="singleTask"
               android:theme="@android:style/Theme.Translucent" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.iapppay.IAppPayHubActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:launchMode="singleTask"
               android:theme="@android:style/Theme.Translucent" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.SelectAmountActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.ui.activity.minipay.BankCardActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"  />
       <activity
               android:name="com.iapppay.ui.activity.normalpay.ChargeActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.pay.channel.gamepay.GamepayActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar"
               android:launchMode="singleTask" >
       </activity>
       <activity
               android:name="com.iapppay.pay.channel.unionpay.UpPayResultActivity"
               android:configChanges="orientation|keyboardHidden"
               android:theme="@android:style/Theme.Translucent" />
       <activity
               android:name="com.unionpay.uppay.PayActivity"
               android:configChanges="orientation|keyboardHidden|screenSize"
               android:excludeFromRecents="true"
               android:label="@string/app_name"
               android:screenOrientation="portrait" />
       <activity
               android:name="com.alipay.sdk.app.H5PayActivity"
               android:configChanges="orientation|keyboardHidden|navigation"
               android:exported="false"
               android:screenOrientation="behind"
               android:windowSoftInputMode="adjustResize|stateHidden" >
       </activity>
       <activity
               android:name="com.iapppay.pay.channel.tenpay.wap.TenpayWapPayActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent" />
       <activity
               android:name="com.iapppay.pay.channel.tenpay.wap.WebActivity"
               android:configChanges="screenSize|orientation|keyboard|navigation|layoutDirection"
               android:theme="@android:style/Theme.Translucent.NoTitleBar" >
       </activity>
       <activity
               android:name="com.iapppay.pay.channel.ecopay2.EcoPay2ResultActivity"
               android:configChanges="orientation|keyboardHidden"
               android:theme="@android:style/Theme.Translucent" />
       <activity
               android:name="com.payeco.android.plugin.PayecoPluginLoadingActivity"
               android:launchMode="singleTask"
               android:screenOrientation="portrait" />
       <activity
               android:name="com.payeco.android.plugin.PayecoCameraActivity"
               android:screenOrientation="portrait" />
       <activity
               android:name="com.payeco.android.plugin.PayecoVedioActivity"
               android:process="com.payeco.android.plugin.vedio"
               android:screenOrientation="landscape" />
	<activity
            android:name="（项目包名）.wxapi.WXEntryActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:launchMode="singleTop"
            android:theme="@android:style/Theme.Translucent" />
        <receiver
            android:name="（项目包名）.AppRegister"
            android:permission="com.tencent.mm.plugin.permission.SEND" >
            <intent-filter>
                <action android:name="com.tencent.mm.plugin.openapi.Intent.ACTION_REFRESH_WXAPP" />
            </intent-filter>
        </receiver>
```

3 接口接入
----------------------------
必须接入的接口：初始化接口、生命周期接口、支付接口、退出接口
可选接口：用户中心接口
###3.1 初始化接口
   在游戏的第一个Activity(非闪屏Activity)中 onCreate() 方法中调用NKBaseSDK.getInstance().init方法,第一个参数是游戏的activity，第二个参数是游戏ID，第三个参数是游戏名称，第四个参数是是否横屏，第五个参数是游戏接收SDK的回调类
  ```
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       NKBaseSDK.getInstance().init(this, "Game1-游戏ID", "我的游戏-游戏名", true,new MyListener());
       NKBaseSDK.getInstance().onCreate();
   }
   ```
###3.2 生命周期接口
   在游戏各个Activity（除闪屏Activity外）生命周期中调用SDK生命周期接口：
   注意，onCreate需要在init之后调用
   ```
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       NKBaseSDK.getInstance().init(this, "Game1-游戏ID", "我的游戏-游戏名", true,new MyListener());
       NKBaseSDK.getInstance().onCreate();
   }
   @Override
   protected void onStop() {
       super.onStop();
       NKBaseSDK.getInstance().onStop();
   }
   @Override
   protected void onDestroy() {
       super.onDestroy();
       NKBaseSDK.getInstance().onDestroy();
   }
   @Override
   protected void onResume() {
       super.onResume();
       NKBaseSDK.getInstance().onResume();
   }
   @Override
   protected void onPause() {
       super.onPause();
       NKBaseSDK.getInstance().onPause();
   }
   @Override
   protected void onStart() {
       super.onStart();
       NKBaseSDK.getInstance().onStart();
   }
   @Override
   protected void onRestart() {
       super.onRestart();
       NKBaseSDK.getInstance().onRestart();
   }
   @Override
   protected void onNewIntent(Intent intent) {
       super.onNewIntent(intent);
       NKBaseSDK.getInstance().onNewIntent(intent);
   }
   @Override
   protected void onActivityResult(int requestCode, int resultCode, Intent data) {
       super.onActivityResult(requestCode, resultCode, data);
       NKBaseSDK.getInstance().onActivityResult(requestCode,resultCode,data);
   }
   ```
###3.3 登录接口
  ```
  NKBaseSDK.getInstance().login("Line1","线/区服名称");
  ```
   参数说明：
     lineID ：游戏默认登录的游戏线（区服）ID，如果没有，可以随意填写，等选择之后再调用selectLine设置正确的游戏线（区服）ID
     lineName ：游戏线（区服）名称
   接口说明：
     初始化成功后才可以调用此接口。
###3.4 支付接口
 ```
 NKBaseSDK.getInstance().pay(payInfo);
  ```
   参数说明：
     payInfo：支付信息结构体
   接口说明：
     初始化成功后才可以调用此接口。支付有结果后可以在init中传入的回调类中处理支付结果
     NKPayInfo结构说明
 ```
               NKPayInfo payInfo = new NKPayInfo();
               //商品价格，单位为分
               payInfo.setAmount(64800);
               //价格与游戏货币的兑换比例，1元对应多少游戏货币
               payInfo.setExchangeRatio(100);
               //游戏货币的名称
               payInfo.setMoneyName("钻石");
               //游戏cp设置的充值透传参数
               payInfo.setExtra("extra");
               //购买的产品id（如果有的话，没有可以不用设置）
               payInfo.setProductId("12345");
               // 购买的产品名称
               payInfo.setProductName("648来一发");
  ```
###3.5 退出接口
``` 
NKBaseSDK.getInstance().quit() 
```  
 接口说明：
     初始化成功后才可以调用此接口。需要根据不同的渠道执行不同的策略
```  
   NKBaseSDK.getInstance().hasQuitPanel()
//返回的是渠道是否有退出界面，如果渠道有退出界面，则游戏不可以弹出自己的退出界面
     
               if (NKBaseSDK.getInstance().hasQuitPanel())
               {
// sdk有退出界面,游戏不要弹出自己的退出确认界面,调用下面的方法会弹出SDK的退出界面，等待sdk的quit回调之后销毁游戏
                 NKBaseSDK.getInstance().quit()
               }
               else
               {
// sdk渠道没有退出界面,游戏可以弹出自己的退出界面,玩家确认退出之后调用NKBaseSDK.getInstance().quit，或者没有退出界面直接调用
//NKBaseSDK.getInstance().quit然后等待sdk的quit回调之后销毁游戏
                 NKBaseSDK.getInstance().quit()
               }
```
###3.6 登出接口
``` 
NKBaseSDK.getInstance().logout()
```  
 接口说明：初始化成功后才可以调用此接口。调用之后会收到onLogout回调通知，再次调用登录界面时不会自动登录
###3.7 用户中心接口
```
 NKBaseSDK.getInstance().userCenter()
```  
 接口说明：初始化成功后才可以调用此接口。调用之后会弹出用户中心界面，如果未登录则是弹出登录界面，cp可以在登录界面或者设置界面添加按钮调用此接口
###3.8 登录成功后角色信息上传接口
``` 
NKBaseSDK.getInstance().roleLoggedIn(String roleID,String roleName,String roleLevel,String lineID,String lineName,String guildName,long roleCT,boolean createRole = false)
   参数说明：
     roleID ：角色ID
     roleName ：角色名称
     roleLevel ：角色等级
     lineID ：线服ID
     lineName ：线服名称
     guildName ：公会名称
     roleCT ：角色创建Unix时间戳（秒）
     createRole ： 是否创建角色操作，默认为false，既不是刚创建的新角色
```   
接口说明：登陆成功后上传角色信息
###3.9 角色升级后角色信息上传接口
``` 
NKBaseSDK.getInstance().roleLevelup(String roleID,String roleName,String roleLevel,String lineID,String lineName,String guildName,long roleCT)
   参数说明：
     roleID ：角色ID
     roleName ：角色名称
     roleLevel ：角色等级
     lineID ：线服ID
     lineName ：线服名称
     guildName ：公会名称
     roleCT ：角色创建Unix时间戳（秒）
```  
 接口说明： 角色升级后上传角色信息
###3.10 微信登陆类型设置（需签名后才能调用）
    1.在Assest下的config.properties 文件中配置wechat_APPID改为自己项目所申请的微信appid，
    2.NKDemo 下面的com.nkgame.demo.wxapi 为微信登录包，在配置时包名需改成项目包名+wxapi  
    WXEntryActivity.java文件复制到该包名下即可
    3.详情可参考：https://open.weixin.qq.com
 
3.11 切换账号
------------------------------------------
```
NKBaseSDK.getInstance().switchAccount(String lineID,String lineName)
```
参数说明：
	lineID ：线服ID
        lineName ：线服名称
接口说明：
     切换账号


4 其他接口
--------------------------
###4.1获取渠道版本号
``` 
NKBaseSDK.getInstance().getVersion()
```  
 接口说明： 返回SDK的版本号（单独接入拟酷渠道时返回拟酷SDK版本号，接入拟酷平台打包全部渠道时返回各自渠道版本号）
###4.2获取渠道ID
``` 
NKBaseSDK.getInstance().getPlatformID();
```   
接口说明： 返回SDK的渠道标识ID（单独接入拟酷渠道时无需使用此方法，接入拟酷平台打包全部渠道时有用）
###4.3渠道是否有用户中心功能
 ```
NKBaseSDK.getInstance().hasUserCenter();
```
   接口说明：返回渠道是否有用户中心功能（单独接入拟酷渠道时无需使用此方法，接入拟酷平台打包全部渠道时有用）
###4.4渠道是否有退出界面功能
```
 NKBaseSDK.getInstance().hasQuitPanel();
```   
接口说明: 返回渠道是否有退出界面功能（单独接入拟酷渠道时无需使用此方法，接入拟酷平台打包全部渠道时有用）

5 回调接口 NKListener
------------------------
json object仅当errorcode 为0时才保证有效，如果errorcode不为0，请直接忽略json object
###5.1 onInit
 调用初始化接口后收到此回调
   参数说明：
     errorcode 为0表示成功，-1表示未知错误,
     json 格式如下
       {"result":0}
###5.2 onLogin
 调用登录接口后收到此回调
   参数说明：
     errorcode 为0表示成功，-1表示未知错误,1表示用户取消（用户取消后请自行再次调用login或者等待用户再次点击登录按钮调用Login）
     uuid字段为拟酷平台转换后用来验证token的用户id，extra字段下的uuid字段为此渠道自身的用户id，如有疑问，请联系技术解释
     extra字段下的loginType中的值表示此次应用宝登录的类型，分别为wechat和qq，全小写
     json 格式如下
```      
 {"result":0,"uuid":"123456789","token":"asdfzxcvasdfqwer","extra":     {"uuid":"b95de17361f01369b2c3534176928b62","loginType":"wechat"}}
```
###5.3 onLogout
 调用登出接口之后收到此回调
   参数说明：
     errorcode 为0表示成功，-1表示未知错误
     json 格式如下
```
       {"result":0}
```
###5.4 onPay
 调用pay之后收到此回调
   参数说明：
     errorcode 为0表示成功，-1表示未知错误,1表示用户取消
     json 格式如下
```   
    {"result":0,"orderID":"nk12345678"}
```
###5.5 onQuit
 调用登出接口之后收到此回调
   参数说明：
     errorcode 为0表示成功，-1表示未知错误
     json 格式如下
```   
    {"result":0}
```
