# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:

Developed by: Shubhavi M
Registration Number : 212223040199

## SERVER APP:
## IColorService:
```
package com.example.aidlserver;

interface IColorService {
    int getRandomColor();
}
```
## ColorService.java:
```
package com.example.aidlserver;

import android.app.Service;
import android.content.Intent;
import android.graphics.Color;
import android.os.IBinder;
import android.os.RemoteException;

import java.util.Random;

public class ColorService extends Service {

    private final IColorService.Stub binder = new IColorService.Stub() {

        @Override
        public int getRandomColor() throws RemoteException {

            Random random = new Random();

            int red = random.nextInt(256);
            int green = random.nextInt(256);
            int blue = random.nextInt(256);

            return Color.rgb(red, green, blue);
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}
```
## AndroidManifest.xml:
```
<service
    android:name=".ColorService"
    android:enabled="true"
    android:exported="true">

    <intent-filter>
        <action android:name="com.example.aidlserver.IColorService"/>
    </intent-filter>

</service>
```
## CLIENT APP:
## activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnChangeColor"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:text="Tap me!"
        android:textSize="24sp" />

</LinearLayout>
```
## MainActivity.java:
```
package com.example.aidlclient;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.example.aidlserver.IColorService;

public class MainActivity extends AppCompatActivity {

    private IColorService colorService;
    private boolean isBound = false;
    private Button btnChangeColor;

    private ServiceConnection connection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            colorService = IColorService.Stub.asInterface(service);
            isBound = true;
            Toast.makeText(MainActivity.this, "Connected", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBound = false;
            colorService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnChangeColor = findViewById(R.id.btnChangeColor);

        btnChangeColor.setOnClickListener(v -> {
            if (isBound && colorService != null) {
                try {
                    int randomColor = colorService.getRandomColor();
                    btnChangeColor.setBackgroundColor(randomColor);
                } catch (RemoteException e) {
                    Toast.makeText(this, "Error", Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(this, "Not connected", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();

        // Bind to the server's service
        Intent intent = new Intent("com.example.aidlserver.IColorService");
        intent.setPackage("com.example.aidlserver"); // Tells Android which app to bind

        bindService(intent, connection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();

        if (isBound) {
            unbindService(connection);
            isBound = false;
        }
    }
}
```
## AndroidManifest:
```
 <queries>
        <package android:name="com.example.aidlserver" />
    </queries>
```
## OUTPUT
<img width="1910" height="1013" alt="Screenshot 2026-08-03 111854" src="https://github.com/user-attachments/assets/13eba494-d344-416d-9b18-e518dc26d1b8" />
<img width="1912" height="1011" alt="Screenshot 2026-08-03 111915" src="https://github.com/user-attachments/assets/bcf76012-3be9-4492-9c4e-4975f3795116" />
<img width="1917" height="1020" alt="Screenshot 2026-08-03 111933" src="https://github.com/user-attachments/assets/0bf94c13-f9c4-41da-a087-6a5c7161fbe6" />


## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
