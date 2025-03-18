# How to Deploy an Express App to Vercel

Vercel makes deploying apps, like Next.js and others, incredibly easy.
And yes this includes your Express API. It does come with a couple
gotchas, though.

All serverless functions in Vercel are run from the api directory. So,
your app will have to be in the api directory.

And, you will need to tell Vercel to direct all requests from the index
of your project to the directory your app lives in. You can do that with
a vercel.json file. Simply create one in the root of your project and
add the following JSON inside.

```json
{
    "rewrites": [{ "source": "/api(.*)", "destination": "/api" }]
}
```

## A Note on Environment Variables

It’s important too, to note that, unlike Next.js and other frameworks,
Express does not automatically read environment variables from your .env
file when running the app locally. To use environment variables in
Vercel, just plug them into the settings and deploy. But to run locally,
use a package like dotenv. Be sure to run the dotenv config inside the
index Javascript file in your api directory, though, not the root. Just
import the following at the top of /api/index.js. (Your entry file may
be named something different.)

```javascript
import "dotenv/config";
```

Then you can use your variables as you normally would.