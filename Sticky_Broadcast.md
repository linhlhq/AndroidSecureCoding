# Sticky Broadcast
Generally, intent receivers should be registered before the intent is broadcasted by other apps. This way, the system knows about the receivers and delivers the intent to the right receivers. Otherwise, the system drops the broadcasted intent like the case where the app is not ready but is interested in a broadcast intent which may get broadcasted even before the app is ready. To address this issue, Android has introduced Sticky Broadcast. These intents stick around after the broadcast is complete. Once the system notices that the app has registered its broadcast receiver, it delivers the intent to the app. The system uses this to broadcast system related events example Intent.ACTION_BATTERY_CHANGED. 

Sticky broadcasts cannot be secured with a permission and therefore are accessible to any receiver. if these broadcasts contain sensitive data or reach a malicious receiver, the application may be compromised. So don't use sendStickyBroadcast() method in your application. Just use LocalBroadcastManager if you want to do intra-application communication.
```
context.sendStickyBroadcast(intent);
```
```
context.sendBroadcast(intent, "permission.ALLOW");
```