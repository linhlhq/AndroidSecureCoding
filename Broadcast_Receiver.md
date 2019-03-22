# Broadcast Receiver

[BroadcastReceivers](https://developer.android.com/reference/android/content/BroadcastReceiver.html) are used to receive an intent and take action on it. This intent is sent by a sender to request some action by the broadcast receiver. If the broadcast receiver is taking some requested action without checking the broadcaster permission, that can cause a security vulnerability.  For example, an application may listen for an SMS broadcast intent to authorize a user based on the SMS. A malicious application can impersonate itself as the system and send the broadcast intent. Since the broadcast receiver is not checking for the proper broadcast permission, it will trust the intent and authorize the user.

If you are exposing broadcast receiver externally, it is really necessary to check for broadcaster permission. In the case you are using a broadcast receiver for intra-app communication, use [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). It is also necessary to check the content of the intent before performing the action as the sender can send bad data which can cause your application to crash. 
If your sole purpose is to use a broadcast receiver for intra-app communication then we would recommend using the [EventBus](https://github.com/greenrobot/EventBus) instead. It is lightweight and you don't have to deal with security stuff.  

 
```
context.registerReceiver(broadcastReceiver, intentFilter);
```
```
context.registerReceiver(broadcastReceiver, intentFilter, "permission.ALLOW_BROADCAST", null);
 # You can also use a local broadcast manager if you intend to broadcast the intent within the app.
LocalBroadcastManager.getInstance(this).registerReceiver(boradcastReceiver, intentFilter)
```