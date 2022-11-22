# Andorid和Js的交互桥梁 ---- WebView
## 一、JS调用Android的方法
三种方法
### **1.1 通过WebView的`addJavaScriptInterface()`进行对象映射** 
#### **1.1.1 具体原理**
Android 和 JS 通过`WebView.addJavaScriptInterface(new JSKit(), "jsKit")`方法形成对象映射,JS中的`jsKit`对象就可以调用JSKit中的方法
#### **1.1.2 具体使用**
* 定义一个与JS映射关系的Android类：JSKit
```kotlin
class JSKit {
    @JavascriptInterface
    fun hello(msg: String) {
        Log.d("MainActivity", "JS成功调用了Android的hello方法")
    }
}
```
* 在Android中通过WebView设置Android类与JS代码的映射
```kotlin
class MainActivity : ComponentActivity() {
    lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        webView = WebView(this)
        setContent {
            ComposeJSDemoTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colors.background
                ) {
                    JSDemo1(webView = webView)
                }
            }
        }
    }
}

@Composable
fun JSDemo1(webView: WebView) {
    Column(modifier = Modifier.fillMaxSize()) {
        AndroidView(
            factory = {
                webView.apply {
                    settings.javaScriptEnabled = true //设置WebView可以与JS交互 这里必须设置
                    settings.javaScriptCanOpenWindowsAutomatically = true // 设置允许JS中的弹窗
                    addJavascriptInterface(JSKit(), "jsKit")

                    loadUrl("file:///android_asset/JSDemo1.html")
                }
            },
            modifier = Modifier.fillMaxSize(),
            update = {webView ->
                webView.webChromeClient = object : WebChromeClient() {
                }
            }
        )
    }
}
```
* 将需要调用的JS代码`JSDemo1.html`放在`src/main/assets`文件夹中
```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <title>Carson</title>  
      <script>
         function callAndroid(){
            // 由于对象映射，所以调用test对象等于调用Android映射的对象
            jsKit.hello("js去调用了android中的hello方法");
         }
      </script>
   </head>
   <body>
      <p>点击按钮则调用callAndroid函数</p>
      <button type="button" id="button1" onclick="callAndroid()">点击调用Android方法1</button>
   </body>
</html>
```

#### **1.1.3 优缺点**
* 有点: 使用简单，仅将Android对象和JS对象映射即可
* 缺点:   
   对于Android 4.2以下，有安全漏洞，需要采用拦截prompt()的方式进行漏洞修复  
   对于Android 4.2以上，则只需要对被JS调用的函数以 @JavascriptInterface进行注解

### **1.2 通过`WebViewClient`的`sholdOverrideUrlLoading()`方法回调拦截url**
#### **1.2.1 具体原理**
Android通过`WebViewClient`的回调方法`shouldOverideUtlLoading()`方法拦截url，解析该url的协议。如果检测到时预先约定好的协议，就调用相应的方法，即JS需要调用Android中的方法
#### **1.2.2 具体使用**
* 在JS约定所需要的Url协议`JSDemo2.html`, 将其放在`src/main/asset`中
```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <title>SoarYuan</title>
     <script>
         function callAndroid(){
            /*约定的url协议为：js://webview?arg1=111&arg2=222*/
            document.location = "js://webview?arg1=111&arg2=222";
         }

         function returnResult(msg) {
            console.debug(msg)
            alert(msg)
         }
      </script>
</head>

<!-- 点击按钮则调用callAndroid()方法  -->
   <body>
     <button type="button" id="button2" onclick="callAndroid()">点击调用Android方法2</button>
   </body>
</html>
```
当JS通过Android的`webView.loadUrl("file:///android_asset/JSDemo2.html")`加载后，就会回调`shouldOverideUrlLoading()`。
* Android代码中，需要复写`shouldOverideUrlLoading`
```kotlin
class MainActivity : ComponentActivity() {
    lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        webView = WebView(this)
        setContent {
            ComposeJSDemoTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colors.background
                ) {
                    JSDemo2(webView = webView)
                }
            }
        }
    }
}
@Composable
fun JSDemo2(webView: WebView) {
    Column(modifier = Modifier.fillMaxSize()) {
        AndroidView(
            factory = {
                webView.apply {
                    settings.javaScriptEnabled = true //  //设置WebView可以与JS交互 这里必须设置
                    settings.javaScriptCanOpenWindowsAutomatically = true // 设置允许JS中的弹窗
                    addJavascriptInterface(JSKit(), "jsKit")

                    loadUrl("file:///android_asset/JSDemo2.html")
                }
            },
            modifier = Modifier.fillMaxSize(),
            update = { webView ->
                webView.webViewClient = object : WebViewClient() {
                    override fun shouldOverrideUrlLoading(
                        view: WebView?,
                        request: WebResourceRequest?
                    ): Boolean {
                        // 根据协议的参数，判断是否是所需要的url
                        // 一般根据scheme（协议格式） & authority（协议名）判断（前两个参数）
                        // 假定传入进来的 url = "js://webview?arg1=111&arg2=222"（同时也是约定好的需要拦截的）
                        var uri = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
                            request?.let { it.url }
                        } else {
                            Uri.parse(request.toString());
                        }
                        uri?.let {
                            // 如果url的协议 = 预先约定的 js 协议,就解析往下解析参数
                            if (it.scheme.equals("js")) {
                                // 如果 authority = 预先约定协议里的webview，即代表都符合约定的协议
                                // 所以拦截url,下面JS开始调用Android需要的方法
                                if (it.authority.equals("webview")) {
                                    // 执行JS所需要调用的逻辑
                                    Log.d("MainActivity", "js调用了Android的方法(Demo2)")
                                    var result = "Android回调给JS的数据为useid=123456"
                                    view!!.loadUrl("javascript:returnResult(\"$result\")");
                                    return true
                                }
                            }
                        }
                        return super.shouldOverrideUrlLoading(view, request)
                    }
                }
            }
        )
    }
}
```
#### **1.2.3 优缺点**
* 优点：不存在addJavascriptInterface()方式的漏洞
* 缺点：JS获取Android方法的返回值复杂。如果JS想要得到Android方法的返回值，只能通过WebView的 loadUrl()去执行JS方法把返回值传递回去，相关的代码如下：
```kotlin
// Android：MainActivity.java
mWebView.loadUrl("javascript:returnResult(" + result + ")");

// JS：javascript.html
function returnResult(result){
    alert("result is" + result);
}
```

### **1.3 通过WebChromeClient的OnJsAlert(), OnJsComfirm(), OnJsPrompt()方法回调拦截JS对话框alert(), confirm(), prompt()方法，对消息message进行拦截**
#### **1.3.1 具体原理**
#### **1.3.2 具体使用**
#### **1.3.3 优缺点**

## 二、Android调用JS的方法