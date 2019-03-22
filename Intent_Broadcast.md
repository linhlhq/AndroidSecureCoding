# Intent Broadcast
In the Android system, an Intent is also used to broadcast some event to notify other components/apps. When broadcasting an intent for inter-application communication, it is necessary to mention the receiver permission, otherwise any malicious application can perform a [man in the middle attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack). The system makes sure that intent is delivered to that broadcast receiver that has the permission. This helps avoid a man in the middle attack.
```
context.sendBroadcast(intent);
```
```
context.sendBroadcast(intent, "permission.ALLOW");
```