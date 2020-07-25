1. 新建一个 Empty Activity 项目

2. 把Unity引擎目录下的classes.jar文件拷贝至Android Studio工程中的libs目录

   <font color="darkgray">\Editor\Data\PlaybackEngines\AndroidPlayer\Variations\mono</font>

   <font color="darkgray">\Release\Classes\classes.jar  ->  Ainfo/app/libs/classes.jar</font>

3. 添加 classes.jar 模块 ( F4 或 右键project视图 -> open module settings）

   <font color="darkgray">Dependencies -> + -> 2 jar dependency -> libs\classes.jar</font>

4. 修改 build.gradle

     <font color="darkgray">apple plugin: 'com.android.application'</font>

     <font color="darkgray">apple plugin: 'com.android.library'</font>

     <font color="darkgray">删除这一行  applicationId 'com.killliu.ap'</font>

5. 修改AndroidMainfest.xml

   ```html
   <?xml version="1.0" encoding="utf-8"?>
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
       package="com.killliu.ap">
       <application
           android:label="ap"
           android:theme="@android:style/Theme.NoTitleBar">
           <activity android:name="com.killliu.ap.MainActivity">
               <intent-filter>
                   <action android:name="android.intent.action.MAIN" />
                   <category android:name="android.intent.category.LAUNCHER" />
               </intent-filter>
           </activity>
       </application>
   </manifest>
   ```

6. 删除 res 目录下的所有资源

7. 修改 MainActivity.java

### 注意

1. Android Studio 的 SDK 版本必须和 unity 的版本一致！！！

   <font color="darkgray">eg. Unity -> Minimum API Level -> Android 4.3 'Jelly Bean' (API level 18)</font>

   <font color="darkgray">Android Studio -> settings -> Android SDK -> Android 4.3(Jelly Bean) --- Installed</font>

2. 参考 Editor\Data\PlaybackEngines\AndroidPlayer\Apk\AndroidManifest.xml