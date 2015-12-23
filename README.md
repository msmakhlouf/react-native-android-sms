# Sms for react-native android

A react native android module to list/send sms.

[![npm](https://img.shields.io/npm/v/react-native-android-sms.svg?style=flat-square)](https://www.npmjs.com/package/react-native-android-sms)

[![GitHub tag](https://img.shields.io/github/tag/msmakhlouf/react-native-android-sms.svg?style=flat-square)](https://github.com/msmakhlouf/react-native-android-sms)

## Setup

There are five steps in the setup process.

1. Install the module via npm.
2. Update `android/settings.gradle` file.
3. Update `android/app/build.gradle` file.
4. Register the module in `MainActivity.java` file. 
5. Rebuild and restart package manager

* Install the module via npm
```bash
 npm i --save react-native-android-sms
```

* Update `android/settings.gradle` file

```gradle
...
include ':react-native-android-sms'
project(':react-native-android-sms').projectDir = new File(settingsDir, '../node_modules/react-native-android-sms')
```

* Update `android/app/build.gradle` file

```gradle
...
dependencies {
    ...
    compile project(':react-native-android-sms')
}
```

* Register the module (in `MainActivity.java` file)

```java
import my.qash.react.SmsPackage;  // <--First, we import import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {

  ......
  private static Activity mActivity = null;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mActivity = this;
    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new SmsPackage(this))      // <------- Then, we add the package
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

     mReactRootView.startReactApplication(mReactInstanceManager, "MyExampleApp", null);

    setContentView(mReactRootView);
  }

  ......

}
```
* Run `react-native run-android` from your project root directory




## Usage

- List all SMS messages matching filters.

```js
var SmsAndroid = require('react-native-android-sms');

/* List SMS messages matching the filter */
var filter = {
    box: 'inbox', // 'inbox' (default), 'sent', 'draft', 'outbox', 'failed', 'queued', and '' for all
    // the next 4 filters should NOT be used together, they are OR-ed so pick one
    read: 0, // 0 for unread SMS, 1 for SMS already read
    _id: 1234, // specify the msg id
    address: '+97433------', // sender's phone number
    body: 'Hello', // content to match
    // the next 2 filters can be used for pagination
    indexFrom: 0, // start from index 0
    maxCount: 10, // count of SMS to return each time
};

SmsAndroid.list(JSON.stringify(filter), (fail) => {
        console.log("OH Snap: " + fail)
    },
    (count, smsList) => {
        console.log('Count: ', count);
        console.log('List: ', smsList);
        var arr = JSON.parse(smsList);
        for (var i = 0; i < arr.length; i++) {
            var obj = arr[i];
            console.log("Index: " + i);
            console.log("-->" + obj.date);
            console.log("-->" + obj.body);
        }
    });

/* 
Each sms will be represents by a JSON object represented below

{
  "_id": 1234,
  "thread_id": 3,
  "address": "2900",
  "person": -1,
  "date": 1365053816196,
  "date_sent": 0,
  "protocol": 0,
  "read": 1,
  "status": -1,
  "type": 1,
  "body": "Hello There, I am an SMS",
  "service_center": "+60162999922",
  "locked": 0,
  "error_code": -1,
  "sub_id": -1,
  "seen": 1,
  "deletable": 0,
  "sim_slot": 0,
  "hidden": 0,
  "app_id": 0,
  "msg_id": 0,
  "reserved": 0,
  "pri": 0,
  "teleservice_id": 0,
  "svc_cmd": 0,
  "roam_pending": 0,
  "spam_report": 0,
  "secret_mode": 0,
  "safe_message": 0,
  "favorite": 0
}

*/


```

- Send an SMS

```js
var SmsAndroid = require('react-native-android-sms');
var text = "Hello ... QASH I AM YOUR FATHER !!!!!";
var addressList = {
    addressList: [
        "+97433------", "+97434------"
    ]
}

SmsAndroid.send(JSON.stringify(addressList), text, (fail) => {
        console.log("OH Snap: " + fail)
    },
    (status) => {
        console.log('Status: ', status);
    });

```

- Delete an SMS (coming soon)

