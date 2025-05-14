# Some Expo and EAS Commands for React Native Development

These are some commands I use very often for developing React Native apps with Expo. Keep in mind some additional packages such as expo-dev-client and expo-updates may be required. You may also need to update your eas.json channels.

## Previewing your App Locally

```
expo start -c
```

The -c flag just clears the cache which I find helpful. In the latest version of Expo it will prompt you to run the development build or inside Expo Go. You can also just add the –dev-client flag to your command if you’d like to run your app in your development build.

## Creating a Dev Build

```
eas build --profile development
```

## Creating a Dev Build for iOS Simulators

```
eas build --profile development-simulator --platform ios
```

This will require a separate “development-simulator” profile with some additional configuration inside your eas.json file.

## ESA Updates

```
eas update
```

Updates are self-explanatory but can be used, also to distribute your app internally whiteout having to run a local server. You will need expo-updates installed for this one.

## Submit your App to the GooglePlay and App Stores

```
eas submit
```

This will bundle your build and send it to the appropriate store for people to download and enjoy!