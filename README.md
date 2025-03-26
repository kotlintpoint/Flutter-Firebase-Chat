# Flutter Firebase Chat Application

## Getting Started

## Add Firebase to Flutter app Documentation 
https://firebase.google.com/docs/flutter/setup

## Set Firebase Storage Rules

```allow read, write: if request.auth != null;```

## Firebase Cloud Messaging Setup
https://firebase.google.com/docs/cloud-messaging/flutter/client

- Push notification for ios can only be tested on ios device

### Steps before running application in ios device

1. flutter clean
2. flutter packages get
3. flutterfire configure
4. Firebase console proejct -> Upload APNs Auth key / Certificate
5. cd ios 
6. pod repo update
7. cd ..
8. flutter build ios
9. run app on ios device 
10. Test Notification


## Send Push Notifications automatically via Cloud Functions
```
const functions = require('firebase-functions');
const admin = require('firebase-admin');

admin.initializeApp();

// Cloud Firestore triggers ref: https://firebase.google.com/docs/functions/firestore-events
exports.myFunction = functions.firestore
  .document('chat/{messageId}')
  .onCreate((snapshot, context) => {
    // Return this function's promise, so this ensures the firebase function
    // will keep running, until the notification is scheduled.
    return admin.messaging().send({
      // Sending a notification message.
      notification: {
        title: snapshot.data()['username'],
        body: snapshot.data()['text'],
      },
      data: {
        // Data payload to be sent to the device.
        click_action: 'FLUTTER_NOTIFICATION_CLICK',
      },
      topic: 'chat',
    });
  });

```
