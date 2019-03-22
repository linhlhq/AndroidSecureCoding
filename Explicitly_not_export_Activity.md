# Explicitly not export Activity
An `Activity` is an application component that provides a screen with which users can interact in order to do something. This activity can also be invoked by an implicit intent if this activity has intent filter. Declaring an intent filter for an activity in the `AndroidManifest.xml` file means that the activity may be exported to other apps. If the activity is intended solely for the internal use of the app and an intent filter is declared then any other app, including malware, can activate the activity for unintended use. It is strongly recommended to explicitly make the activity non-exportable by setting the `exported` flag to `false`. Remember in the software world, we should make things explicit instead of implicit.
```
<activity  android:name=".YourInternalActivity"
android:configChanges="keyboard|keyboardHidden|orientation" >           
    <intent-filter >
        <action android:name="com.some.vulnerable.ACTION_UPLOAD" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:mimeType="image/*" />          
    </intent-filter>        
</activity>
```
```
<activity  android:name=".YourInternalActivity" 
          android:configChanges="keyboard|keyboardHidden|orientation"
          android:exported="false" >           
    <intent-filter >
        <action android:name="com.some.vulnerable.ACTION_UPLOAD" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:mimeType="image/*" /> 
    </intent-filter>        
</activity>
```