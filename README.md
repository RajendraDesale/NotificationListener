# NotificationListener
 A notification listener service allows the Google App to intercept notifications posted by other applications. Notification Options in the Google App Notification Listener Services. Within the Android Manifest file is the inclusion of the new Notification Listener Service

below all steps help to register notification service to your app.

1 - put below code in Activity OnCreate Method.
```
 try {
      boolean temp = isNotificationServiceRunning();
            if (!temp) {
                showNotificationEnableAlertDialog("Please enable notification access", "Enable", "Exit", temp);
            } else {
                showNotificationEnableAlertDialog("Please enable notification access", "Enable", "Exit", temp);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
```
2 - create showNotificationEnableAlertDialog() method in your activity for popup.
```
 public void showNotificationEnableAlertDialog(String msg, String yesbtnText, String nobtnText, boolean isEnable) {
        AlertDialog.Builder builder = new AlertDialog.Builder(HomeActivity.this);
        builder.setTitle("Alert!");
        builder.setMessage(msg);
        builder.setPositiveButton(yesbtnText, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                getApplicationContext()
                .startActivity(new Intent("android.settings.ACTION_NOTIFICATION_LISTENER_SETTINGS")
                .addFlags(Intent.FLAG_ACTIVITY_NEW_TASK));
                dialogInterface.dismiss();
            }
        });

        builder.setNegativeButton(nobtnText, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                finish();
                Intent homeIntent = new Intent(Intent.ACTION_MAIN);
                homeIntent.addCategory(Intent.CATEGORY_HOME);
                homeIntent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
                startActivity(homeIntent);
                dialogInterface.dismiss();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.setCancelable(false);
        alertDialog.show();

        if (isEnable && alertDialog.isShowing()) {
            alertDialog.dismiss();
        }
    }
```
3 - create isNotificationServiceRunning() method in your activity for checking if notification service is on or off for your app.
```
    private boolean isNotificationServiceRunning() {
        ContentResolver contentResolver = getContentResolver();
        String enabledNotificationListeners = Settings.Secure.getString(contentResolver, "enabled_notification_listeners");
        String packageName = getPackageName();
        return enabledNotificationListeners != null && enabledNotificationListeners.contains(packageName);
    }
```
4 - create blank notificationService class inside your service folder by extending NotificationListenerService with @override method.

5. Final step just register notificationService service in Manifest.xml file.
```
        <service
            android:name=".services.NotificationService"
            android:label="Notification service"
            android:permission="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE">
            <intent-filter>
                <action android:name="android.service.notification.NotificationListenerService" />
            </intent-filter>
        </service>
```
