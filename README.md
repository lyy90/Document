一、html中通过js调用java代码
1.Webview 提供两种办法和原生交互：1. 路由拦截 2. 原生和JS 交互。

android针对原来项目存在通过路由拦截问题，android 部分机型会出现crash 问题，替换掉原来路由拦截的情况，用原生Js 交互方式，来实现。

2.  测试在2.5.0 反馈一个问题，当webview 加载页面的时候，忽然点返回的键，手机app 崩溃(锤子手机），
个人建议，html5 针对手机端js 交互问题，最好是用原生的导航栏去控制去webview 的返回，
如果不这样，如果不这样控制的话，webview 加载完，点webview 内置返回键，webview 并没有销毁，
如果原生控制的话，可以控制页面关闭和销毁webview 等情况，这样子会避免crash 问题。
3.  WebView和js的交互包含两方面，一是在html中通过js调用安卓的java代码；二是在安卓java代码中调用js。
一、html中通过js调用java代码
js中调用java代码其实就记住一点，webview设置一个和js交互的接口（注意这里只是一般的意思，并不是java中接口的含义），
这个接口其实是一个一般的类，同时为这个接口取一个别名。这个过程如下：
product_webview.addJavascriptInterface(new YMCJavaScriptInterface(), "controller");
new  YMCJavaScriptInterface就是这个接口，controller就是这个接口的别名。
上面的代码执行之后在html的js中就能通过别名（这里是“controller”）来调用newDemoJavaScriptInterface类中的任何方法
 //android 客户端
class YMCJavaScriptInterface {
            YMCJavaScriptInterface () {
            }
            @JavascriptInterface
            public void LYQSKBAPP_OpenShareProduct() {
                   mHandler.post(new Runnable() {
                          public void run() {
                          }});}}
//js调用原生APP分享      js端
function appShareProduct() {
if ("<%= wx.Tool.ToolBox.GetMobileRequestType()%>"== "<%=wx.Core.MobileRequestType.AndroidSkbApp %>") {
     controller.LYQSKBAPP_OpenShareProduct(); //让APP打开分享界面     android 
}
else {
document.location = "objectc:LYQSKBAPP_OpenShareProduct";//让APP打开分享界面 //window.open("objectc:LYQSKBAPP_OpenShareProduct");//让APP打开分享界面      ios
}}
二、android调用js
上面的代码在演示如何在js中调用java代码的同时也演示了如何在java中调用js
调用形式：
1.调用无参构造函数
mWebView.loadUrl("javascript:wave()");   //直接调用无参构造的函数
其中wave（）是js中的一个方法，当然你可以把这个方法改成其他的方法，也就是android调用其他的方法。
2.调用有参构造函数
1. product_webview.addJavascriptInterface(new YMCJavaScriptInterface(), "controller");
2. function wave(String value) {return value;}
3. class YMCJavaScriptInterface {
   YMCJavaScriptInterface () {}
   @JavascriptInterface
   public void wave (String value) { if(value.equals(“1”))) //code
   else if(value.equals(“2”)){ //code
}}}
