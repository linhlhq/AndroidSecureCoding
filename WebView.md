# WebView
The [WebView](https://developer.android.com/reference/android/webkit/WebView.html) class displays web pages as part of an activity layout. The behavior of a WebView object can be customized using the WebSettings object, which can be obtained from WebView.getSettings(). 

Major security concerns for WebView are from the setJavaScriptEnabled(), setPluginState(), and setAllowFileAccess() methods.

`addJavascriptInterface()` method

For API level JELLY_BEAN or below, allowing an app to use the addJavascriptInterface method with untrusted content in a WebView leaves the app vulnerable to scripting attacks using reflection to access public methods from JavaScript.  Untrusted content examples include content from any HTTP URL (as opposed to HTTPS) and user-provided content. The method addJavascriptInterface(Object, String) is called from the WebView class. Sensitive data and app control should not be exposed to scripting attacks.
```
WebView webView = new WebView(this);
setContentView(webView);
...
class JsObject {
     private String sensitiveInformation;
 
     ...
     public String toString() { return sensitiveInformation; }
 
}
 webView.addJavascriptInterface(new JsObject(), "injectedObject");
 webView.loadData("", "text/html", null);
 webView.loadUrl("http://www.example.com");
<manifest>
<uses-sdk android:minSdkVersion="17" />
...
 
</manifest>
# OR
WebView webView = new WebView(this);
setContentView(webView);
```
`setJavaScriptEnable()` method
 

WebView also provides a method to enable or disable javascript for the web pages. Since WebView displays external webpages, the application has less control over the content. Inherently javascript comes with [cross site attack](https://en.wikipedia.org/wiki/Cross-site_scripting) (XSS) vulnerabilities if not enough care has been taken while creating the web page.  Your code should not invoke setJavaScriptEnable if you are not sure that the web page really requires JavaScript support.
```
public class UnSecureBrowser extends Activity {
 @override
 public void onCreate(Bundle savedInstanceState) {
 super.onCreate(savedInstanceState);
 setContentView(R.layout.main);
 
 WebView webView = (WebView) findViewById(R.id.webview);
 
 
 // turn on javascript
 WebSettings settings = webView.getSettings();
 settings.setJavaScriptEnabled(true);
 // Get the url from Intent and launch in the webview.
 webView.loadUrl(getIntent().getStringExtra("URL"););
 }
}
// If the above Activity is externally exposed, a malicious application can send an
// intent with a locally stored file which will be loaded in the webview.
  
String pkg = "some.unsecure.app";
String cls = pkg + ".UnSecureBrowser";
String filePath = "file:///<LOCALLY_STORED_FILE>";
Intent intent = new Intent();
intent.setClassName(pkg, cls);
intent.putExtra("URL", filePath);
this.startActivity(intent);
setJavaScriptEnabled(false);
```
If your application receives the url form outside a trust-boundary, it should be validated before rendering it with WebView. For example, the following code checks a received URI and uses it only when it is not a file: scheme URI. 
```
String intentUrl = getIntent().getStringExtra("url");
String localUrl = "about:blank";
if (!intentUrl.startsWith("file:")) {
 loadUrl = intentUrl;
}
```