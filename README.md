# view-shopify
Xenon View Instrumentation for Shopify in an extension-only app

### Prerequisites

Before you begin, you'll need the following:

1. **Node.js**: [Download and install](https://nodejs.org/en/download/) it if you haven't already.
2. **Shopify Partner Account**: [Create an account](https://partners.shopify.com/signup) if you don't have one.
3. **Test Store**: Set up either a [development store](https://help.shopify.com/en/partners/dashboard/development-stores#create-a-development-store) or a [Shopify Plus sandbox store](https://help.shopify.com/en/partners/dashboard/managing-stores/plus-sandbox-store) for testing your app.

### Setup

Add the following to the theme.liquid file, right near the end of the </head> section
```
    <!-- begin Xenon capture -->
    <script>
      document.addEventListener('DOMContentLoaded', function() {
        Shopify.analytics.publish('xenon', {timing: JSON.stringify(window.performance.timing)});
      }, false);
    </script>
    <!-- end Xenon capture -->
```

The extension-only Shopify app is called xenon-view.  It has an extension called xenon-capture.

Using npm:

```shell
cd xenon-view
npm install
```

If you updated the version of the app from Git, you need to deploy that update

```shell
npm run deploy
```

### Local Development

```shell
npm run dev
```

Press P to open the URL to your app. Once you click install, you can start development.

Local development is powered by [the Shopify CLI](https://shopify.dev/docs/apps/tools/cli). It logs into your partners account, connects to an app, provides environment variables, updates remote config, creates a tunnel and provides commands to generate extensions.

### Configure the xenon-capture extension

In order for the extension to access the store, you need to run a GraphQl command to activate it.
Go to http://localhost:3457/graphiql and run the following:

```
mutation {
  # Creates a web pixel, and sets the Xenon `apiKey` for this shop
  webPixelCreate(webPixel: { settings: "{\"apiKey\":\"YOUR_API_KEY_HERE\"}" }) {
    userErrors {
      code
      field
      message
    }
    webPixel {
      settings
      id
    }
  }
}
```

You will then see each event logged to the browser console on your development store.

## Resources

- [Introduction to Shopify apps](https://shopify.dev/docs/apps/getting-started)
- [App authentication](https://shopify.dev/docs/apps/auth)
- [Shopify CLI](https://shopify.dev/docs/apps/tools/cli)
- [App extensions](https://shopify.dev/docs/apps/app-extensions/list)
- [Shopify Functions](https://shopify.dev/docs/api/functions)
