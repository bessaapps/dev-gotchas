# How to Automatically Update Users to the Latest Version of an Expo App

The eas-cli tool offers a really great way to make live updates to your app. Say you squash a bug and want to publish the changes to people who have your app installed. You would just run a simple Expo update. Next time they open your app they&apos;ll see the changes reflected. But sometimes, with new packages, infrastructure changes, or just larger updates, you&apos;ll want to publish a new version through the Apple App Store and/or Google Play. It&apos;s important to keep your users on the most up-to-date version of your app. Luckily Expo offers a way to automatically compare the app version with the most latest one in the app stores called [Expo Updates](https://docs.expo.dev/versions/latest/sdk/updates).

First install the Expo Updates package:

```
expo install expo-updates
```

Then, import the package:

```javascript
import * as Updates from "expo-updates"
```

Now you can check for updates and give the option to install them like this:

```javascript
const fetchUpdate = async () => {
    const update = await Updates.checkForUpdateAsync();
    
    if (update.isAvailable) {
        Alert.alert(
            "Time for an Upgrade!",
            "To use the latest and greatest features, update your app.",
            [
                {
                    text: "Update",
                    onPress: async () => {
                        await Updates.fetchUpdateAsync();
                        await Updates.reloadAsync();
                    },
                    isPreferred: true,
                },
                {
                    text: "Later",
                },
            ],
        );
    }
};

useEffect(() => {
    fetchUpdate();
}, []); 
```