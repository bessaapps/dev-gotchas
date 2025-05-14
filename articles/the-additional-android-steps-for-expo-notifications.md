# The Additional Android Steps for Expo Notifications

Sending Push Notifications with Expo Notifications is pretty easy with the power of device tokens. However, there are some gotchas, especially for Android.

## Getting Started

After the iOS and Android dev builds are all set, install [Expo Notifications](https://docs.expo.dev/versions/latest/sdk/notifications) to ask the user for permissions and require the device's token to later send push notifications to.

Use the following to install Expo Notifications. The example in the documentation uses some other packages, too, so it's worth a read.

**Generate new, non-emulator dev builds after installing the packages.**

```
npx expo install expo-notifications
```

```javascript
import * as Notifications from "expo-notifications";
```

Next, run the EAS CLI to make a new build. It will prompt to set up iOS push notifications. You just log in with app store credentials. Easy!

## Android Gotchas

Expo Notifications uses Firebase Cloud Messaging (FCM) to send push notifications to Android devices. This requires a Google service account and the corresponding credentials.

It's possible to get the FCM V1 service account key from Firebase. Create a Firebase project and create an Android App from the project dashboard. Click "Add app" and select the Android Logo. Enter the package name, which is usually in the app.json file. Run through the rest of the prompts and ***be sure to download the google-services.json file, add it to the root of the project, and commit it to version control***. It's needed later. Now download the private key which will be uploaded to Expo. Just go the settings for the new Android app in Firebase. It should be a cog icon. Click "Service accounts" and the button that says, "Generate new private key." This should download your key automatically as a JSON file.

Now you need to upload your private key JSON file to Expo. Login to your Expo dashboard and select your project. Click "Credentials"from the left then select your Android application identifier. You can upload the JSON file you download under the "FCM V1 service account key" section.

Lastly we'll tell Expo where to fine that google-services.json file. Head over to your app.json file and add the following under the Android section. It should look something like this:

```json
{
  "android": {
    "googleServicesFile": "./google-services.json"
  }
}
```

## Troubleshooting

You may need to add FCM permissions to your Firebase service account from the IAM section in Google Cloud. I think Firebase does this for you, though.