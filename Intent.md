# Intent
[Intents](https://developer.android.com/reference/android/content/Intent.html) are mainly used for inter-process communication. Intent can be of two types, implicit and explicit. An implicit intent specifies an action that can invoke any app which can perform this action. It is quite possible that there could be many apps/components that can perform the same action. In that case, the system provides options to the user and the user can then select which application they want to perform the task.

An explicit intent can be used to launch any component of the app. In this case, you explicitly specify the component name while creating the intent. If your intention is to invoke your application component then it is highly recommended to use explicit intents since implicit intents can be intercepted by any other application if the correct receiver permission is not used. The best option is to use explicit intents with LocalBroadcastManager. This is efficient because the intent will never leave the application boundary and hence the system doesn't have to handle the overhead related with systemwide broadcasts.
```
Intent intent = new Intent("com.sample.action.authorize");
intent.putExtra("userid", userId);
intent.putExtra("password", password);
 sendBroadcast(intent);
 ```
 ```
Intent intent = new Intent(context, ConnectionManager.class);
intent.putExtra("userid", userId);
intent.putExtra("password", password);
LocalBroadcastManager.getInstance(this).sendBroadcast(intent);
```
# Pending Intent
 

A [PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html) is an intent that can be given to another application for it to use later. The application receiving the pending intent can perform the operation(s) specified in the pending intent with the same permissions and the same identity as the application that produced the pending intent. Consequently, the pending intent should be built with care and must always contain base intents that have the component name set explicitly to a component owned by the originating application. This ensures that the base intents are ultimately sent to appropriate locations and nowhere else. An implicit intent must never be included in a pending intent.
```
Intent intent = new Intent("implicit.ACTION");
PendingIntent pendingIntent = PendingIntent.getActivity(this, 1, intent, PendingIntent.FLAG_CANCEL_CURRENT);
try {
    pendingIntent.send();
} catch (PendingIntent.CanceledException e) {
    e.printStackTrace();
}
```
```
Intent intent = new Intent(this, LoginActivity.class);
PendingIntent pendingIntent = PendingIntent.getActivity(this, 1, intent, PendingIntent.FLAG_CANCEL_CURRENT);
try {
    pendingIntent.send();
} catch (PendingIntent.CanceledException e) {
    e.printStackTrace();
}
```