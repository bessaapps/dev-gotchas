# Before Getting Started with Firebase Storage Inside Next.js

## CORS

Be sure to update your bucket’s CORS policies to allow connecting from your origin via the http request methods you are using.

First, install the gcloud SDK. Then run the following to login.

```
gcloud init
```

Then navigate to the directory where your preferred CORS JSON settings are stored. Then run the following and replace cors.json with the filename of your cors JSON.

```
gsutil cors set cors.json gs://<project>.appspot.com
```

## Bucket Permissions

In your Firebase console navigate to Storage -&gt; Rules. Use the following settings, replacing &lt;project slug&gt; with your project slug.

```javascript
service firebase.storage {
    match /b/<project>.appspot.com/o {
        match /{allPaths=**} {
            // Allow access by all users
            allow read, write;
        }
    }
}
```