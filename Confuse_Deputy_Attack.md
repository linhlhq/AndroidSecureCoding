# Confuse Deputy Attack
[Confuse Deputy Attack](https://en.wikipedia.org/wiki/Confused_deputy_problem) is when another application misuses your permission. For instance, your application has delete SMS permission and it has exposed an interface through which SMS can be deleted. If your application is not checking the requester permission, it can delete the SMS even when other applications don't have this permission.

The [checkCallingOrSelfPermission](https://developer.android.com/reference/android/content/Context.html#checkCallingOrSelfPermission(java.lang.String)) method can cause this issue so it is really important to check the caller permission before performing the privileged operation. 
```
checkCallingOrSelfPermission(this, "permission.ALLOW");
```
```
checkCallingPermission(this, "permission.ALLOW");
```