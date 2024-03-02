pub
---
tags: App, Frontend, IOS, Setup
---

![[Pasted image 20230803213629.png]]

## Types of Deep Links

iOS provides support for 2 different types of deep links: URL schemes and universal links. URL schemes allow you to create your own scheme that only corresponds to actions in your app. For example, you can have something like **myapp://settings** to bring users to the settings page in your app. The big advantage here is that these custom schemes can _only_ be used to open your app, superseding any OS logic for determining if your app should open. However, this is also its biggest disadvantage. If someone doesn’t have your app installed and taps on one of your URL schemes, iOS will display a nasty error saying that it can’t open the URL. There’s no indication that they should download your app to continue. This creates a suboptimal experience for potential would-be users who encountered your link. Additionally, URL schemes are highlighted by Apple as being potentially susceptible to security concerns, so they highly recommend avoiding them, if possible.

Universal links look just like any regular URL that you know and are used to. For example, it could look something like [**https://cmx.weightwatchers.com/nui/my-day**.](https://cmx.weightwatchers.com/nui/my-day.) The advantage here is that since this is just a standard URL, iOS will open Safari and navigate to that page if the user doesn’t have your app installed. This is a far better UX than a non-descriptive error message.

The rest of this article will focus exclusively on universal links, since they are recommended more than URL schemes and contain some complexities that are worth explaining.

## How Does iOS Know to Open Your App?

Before diving into the actual setup process, it’s important to go over how iOS determines to open your app instead of Safari when a universal link is tapped. There is a lot of complexity going on here to provide different experiences, depending on your situation.

When a user installs your app, iOS checks the app’s manifest for its associated domains (provided by you in Xcode) and specifically checks for domains beginning with **applinks:**, since these indicate a domain for deep linking. iOS will then take each matching domain and hit the **/.well-known/apple-app-site-association** path for those domains. This path should contain a specifically structured JSON file provided by you, which is explained more in-depth later. The JSON file contains information about which paths for that domain are valid deep links. iOS will store these valid paths in a local manifest containing all valid deep links for all installed apps. Note that this process only happens in two instances: when your app is first installed and for each subsequent update. If you decide to update the valid paths, you will need to release a new update to force iOS to re-check that JSON file.

When any link is tapped from a valid context, iOS will first check its local deep link manifest to see if it matches any known deep links. If a match is found, the URL is routed to that app to handle. Otherwise, the link opens normally in Safari. This process happens quickly behind the scenes and is totally hidden from the user.

## Server configuration
We need to upload a JSON file called `apple-app-site-association` on the server which will define the type of links that will act as universal links for the iOS App.

1. Name of the file should be `apple-app-site-association`
2. The `apple-app-site-association` file is added under `.well-known` subdirectory of the website hosting it. The address of the file should be `/.well-known/apple-app-site-association`
3. The file should be a valid JSON file without any extension and is served from an `HTTPS` address. This means you do not need to append `.json`to the `apple-app-site-association` filename.
4. If you plan on supporting multiple domains or subdomains then there should be a separate `apple-app-site-association` file for each domain with unique content that your app supports. Also, each domain and subdomain requires its own entry in the `[[Associated Domains Entitlement]](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_associated-domains)`
5. The uncompressed file size should be no greater than 128 KB
6. The file should have MIME type as `application/json`
7. Before adding data to the file, gather the following so you have it ready when you create your `apple-app-site-association` file.

- **Team ID** which is the same value that’s associated with the “application-identifier” key in your app’s entitlements after you build. This can be found under [developer.apple.com](http://developer.apple.com/) portal → Team ID
- **Bundle identifier** which can be found in XCode under your target → scheme

8. Below is a sample `apple-app-site-association` file with explanation of what each items means

- `appID` is a combined string value of TeamID with Bundle Identifier of your app. So an with team id as ABCD1234 and bundle identifier as `com.apple.wwdc` will have `appID` in the file as ABCD1234.com.apple.wwdc
- `*` and `?` are wildcard in the pattern strings
- `*` matches 0 or more characters and in terms of paths if we only have `*` then it means having a universal link for your entire website. Anytime a user opens your website it will be prompted to open the app.
- `?` matches exactly any one character
- `*` will allow us to match at least one character
- `/` represent specific path if they are not followed by `*` symbol. This means when a user goes /wwdc/news/ it will prompt to open the the app.

9. In recent years Apple has also introduced another version of `apple-app-site-association` file which is specific to iOS 13.5 and above with more options to configure. However, the older version should still work. Below is an example of contents of the newer version of apple-app-site-association file.

```json
{
  "applinks": {
      "details": [
           {
             "appIDs": [ "ABCDE12345.com.example.app", "ABCDE12345.com.example.app2" ],
             "components": [
               {
                  "#": "no_universal_links",
                  "exclude": true,
                  "comment": "Matches any URL with a fragment that equals no_universal_links and instructs the system not to open it as a universal link."
               },
               {
                  "/": "/buy/*",
                  "comment": "Matches any URL with a path that starts with /buy/."
               },
               {
                  "/": "/help/website/*",
                  "exclude": true,
                  "comment": "Matches any URL with a path that starts with /help/website/ and instructs the system not to open it as a universal link."
               },
               {
                  "/": "/help/*",
                  "?": { "articleNumber": "????" },
                  "comment": "Matches any URL with a path that starts with /help/ and that has a query item with name 'articleNumber' and a value of exactly four characters."
               }
             ]
           }
       ]
   },
   "webcredentials": {
      "apps": [ "ABCDE12345.com.example.app" ]
   },


    "appclips": {
        "apps": ["ABCED12345.com.example.MyApp.Clip"]
    }
}
```

**Note**: For iOS 14 and above, Apple CDN will fetch the `apple-app-site-association` file from the server as long as it is behind an HTTPS connecting and configured properly. Look for **_Apple’s Bot (“AASA-Bot/1.0.0”)_** to read the change in your server console logs. This can take anywhere from few minutes to a few hrs to get the change.


## References
https://medium.com/ww-tech-blog/a-deep-dive-into-ios-deep-linking-8fd8f4a00f91
https://nishbhasin.medium.com/apple-universal-link-setup-in-ios-131a508b45d1
