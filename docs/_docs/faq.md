---
title: FAQs
order: 16
layout: post-toc
redirect_from: /docs/
---

# Frequently Asked Questions

<a id="code"></a>
## How Does Code Mode Work?

![Zapier visual builder code mode](https://cdn.zapier.com/storage/photos/5abf045cf0b8f3cce37d05d51071d6e9.png)
_Each API call pane in Zapier visual builder includes a code mode toggle_

Zapier visual builder includes a form to add API endpoint URLs and choose the API call type, then automatically includes any authentication details and input form data with the API call. You can additionally set any custom options your API may need, including custom URL params, HTTP headers, and request body items. Zapier then parses JSON encoded responses into individual output fields to use in subsequent Zap steps.

This is the best way to set up most API calls and options in Zapier integration authentication, triggers, and actions.

If your API calls need more customization, however, or your API response is in a non-JSON format, you will need to write custom JavaScript code to handle your API call and/or response parsing. Zapier visual builder includes a _Switch to Code Mode_ toggle on every API request, similar to the one pictured above, that switches that specific API call to code mode.

> **Remember:** Code Mode is a toggle. Code Mode and Form Mode are saved separately; changes to one do not affect the other. The settings in the currently visible editor are the ones Zapier uses in your integration.

The first time you switch to code mode, Zapier copies everything entered in the API request form, including any custom options added, and converts them to JavaScript code. It then changes the UI to code mode where you can add code for your API call.

In code mode, you can write JavaScript code, using Zapier's default code as a base or writing custom code. Use the `z.object` for Zapier specific features, including `z.console` to write to the console log, `z.JSON` to parse JSON, `z.errors` to take action on errors, and more. Check [Zapier's CLI Z Object docs](https://zapier.github.io/zapier-platform-cli/#z-object) for details.

Additionally, use Zapier bundles to access auth data, data from user input forms, request data, and more. Learn more in our [Zapier bundle docs](https://zapier.github.io/visual-builder/docs/advanced#bundle).

Do note that changes are not saved automatically. Once you have added the code you want, click _Save & Continue_ to add the changes to your integration.

![Zapier code mode switch to form mode](https://cdn.zapier.com/storage/photos/ea2eb690bf92b55fab0bbad290107a97.png)
_Switch back to form mode to use the form mode settings instead of your custom code_

Once you switch to code mode, Zapier uses your custom code for that API call, and does not use the data you previously entered in the form.

If you wish to switch back to the form mode, click the _Switch to Form Mode_ button to see the form options as they were when you first switched to code mode. Zapier will save the code you entered, but will not convert it back to the form mode or use the custom code in your live integration.

You may switch back to code mode again—though this time, Zapier will show the last saved version of your code, and will not copy any changes from your API call form to the code.

## Is my Zapier Integration Using the Form or Code Mode Settings?

Zapier uses the currently visible option when running each part of your integration. If the form mode is visible for an API call in an integration's authentication, trigger, or action settings, that Zapier integration is using the data from form mode. If the code mode is visible for an API call, Zapier is using the code instead to turn that part of the integration.

To check which mode and settings Zapier is using for each API call, open that part of your Zapier integration and visually check to see if the form or code mode is visible.

<a id="cli"></a>
## Why Are Options Grayed Out For my CLI-built Integration?

![Zapier CLI integration in visual builder](https://cdn.zapier.com/storage/photos/c631ca2cd91ab43b0bc4d22f641eb4d6.png)

The Zapier Command Line Interface (CLI) is a separate SDK available to install on your local development machine to create Zapier integrations. It lets you work in code rather than a web based UI for more advanced integrations.

Integrations created in the CLI cannot be edited in the visual builder UI. You can’t add triggers or actions, edit code or configurations, for instance.  Zapier's platform site lists every integration you build, in the visual builder or CLI, but disables the core editing options for CLI-built integrations. These options will be grayed out in the UI.

You _can_ manage the other details of your CLI integration from the UI, however, including:

- Invite testers and collaborators
- Monitor integration usage
- Change environment variables
- Submit your integration to be made publicly available
- Promote a new integration version to public
- Migrate users between versions

_Coming Soon_: In an upcoming version of Zapier's visual builder, you will be able to export a CLI version of your visual builder project. This will be a one-time export that converts your visual builder integration to a CLI format that you can edit and maintain on your local development machine. You can then create and push new versions of your integration via Zapier CLI, and can manage the details from the visual builder UI or the CLI. Once you enable CLI, though, you will not be able to edit or add authentication, trigger, or action details in the visual builder UI.

<a id="array"></a>
## I get a trigger error saying that an array is expected. How do I fix it?

When you configure a polling trigger, the Zapier platform expects to get a bare array of the trigger's items, sorted in reverse chronological order. But very often, APIs will return a result _object_ that contains the array of items the trigger needs.

Let's look at an example. For a "New Channel" trigger with Slack's API, we might start with a request that looks like this:

![](https://cdn.zapier.com/storage/photos/1f84a3519ad4e78c3567cdbff8a4c1d3.png)

When we test it we get this message:

![](https://cdn.zapier.com/storage/photos/0b652f00538c655111e444a0c4d35ab0.png)

Digging into the API response we can see that what was returned was an _object_ that contains the array of items we need, not the array itself:

```
{
    "ok": true,
    "channels": [
        {
            "id": "01234",
            "name": "general",
            "is_channel": true,
            "created": 1390943394,
            "is_archived": false,
            "is_general": true,
            "unlinked": 0,
            ....
```

We can see that what we need to return is that array of channels. To do that we need to switch over to "Code Mode" in our request. That lets us provide a JavaScript function to handle our request. There we can make any needed changes to the structure or content of the result before we return data to the Zapier platform. Notice line 18:

![](https://cdn.zapier.com/storage/photos/e1d09e9fdf952ef5f5b031bd705e5802.png)

> Remember that "Form Mode" vs "Code Mode" is a mutually exclusive toggle. If you switch back to Form Mode your code will be ignored!

We can now retest the request and see that it's successful. Note the response is a bare array:

![](https://cdn.zapier.com/storage/photos/33098c9f9c1584295e074c1dc8a40e72.png)

<a id="cleanup"></a>
## How to Clean Up Test Authentication Accounts?

![Example account with multiple test accounts](https://cdn.zapier.com/storage/photos/7fe9f9155dafbcc5d9b461720d664d4d.png)

While building your integration, testing authentication, and customizing your app's connection label, you may quickly end up with a list of broken or old app authentications. You can clean those up and remove older accounts from your core Zapier account.

![Remove authed accounts](https://cdn.zapier.com/storage/photos/c7fe45e90f7c4c00f6ffd938d417cdd0.png)

Open your [Zapier _Connected Accounts_](https://zapier.com/app/settings/authorizations) page, and press `CMD`+`F` or `Ctrl`+`F` then search for your app's name. Click _Disconnect_ then confirm to remove any account, and repeat for each subsequent testing account you added to clean up your authentication list.

Then refresh your integration page in visual builder, and you'll only see the authentications you left running.

<a id="output"></a>
## How do Sample Data and Output Fields Work?

![Adding Sample Data to Zapier integration](https://cdn.zapier.com/storage/photos/8ab32f061aa89f3b57e8f4a5ea16a9d9.png)
_Sample Data gives Zapier example data if users don't test the trigger or action; Output Fields give your API data user-friendly names in subsequent Zap steps_

The last step in creating a new Trigger or Action for a Zapier integration is to _Define your Output_. Here, Zapier asks both for Sample Data and Output Fields. Neither are required, but both help improve the Zapier experience for your users.

![Sample Data in a Zap Step](https://cdn.zapier.com/storage/photos/d4e5d47c461efa897a907c2806aecc1d.png)

Sample Data lists the default data Zapier shows users when building a Zap using this step. By default, Zapier will ask to test the Zap step after users set it up. With Triggers, Zapier will try to fetch recently added or updated items; with Actions, Zapier will try to find or create the item using the user's data.

If users are in a hurry, though, they can click _Skip This Step_. Zapier will then show the sample data instead, both with triggers and actions, so they can map fields correctly in subsequent Zap steps without seeing their app account's live data. Or with triggers, if your app doesn't have any data for this item yet, Zapier will default to showing the sample data instead of showing an error that no items are available.

Sample Data is especially important for Triggers, and useful with Actions as well.

To include sample data in your integration, open the final step of your Zapier trigger or action's API Configuration. There, write valid JSON output data using the same field names as your API with sample response data that does not include any personally identifiable data, is safe for work copy, and is easily recognizable as sample data. Here are example sample data for a GitHub issue:

    {
      "body": "This is a sample issue",
      "html_url": "https://github.com/",
      "title": "Test Issue"
    }

![Adding Output Fields to Zapier integration](https://cdn.zapier.com/storage/photos/31d6247a888135ed334d5035ce4b0ade.png)

Output Fields add user-friendly labels to your API's response data. By default, Zapier will try to make friendly version of your API's response, capitalizing words and adding spaces instead of underscores. You can customize this further with Output Fields.

For example, if you use GitHub's API to watch for new issues, the API calls the issue name _Title_. Users may expect that field to be called _Issue_ or _Issue Title_, so you could define that add a custom name for that field.

Output Fields are equally important for both triggers and actions, as the output data from both may be used in subsequent Zap steps.

To add output fields, list the original field name from your API response in the left column, and a human friendly name for the field in the right column of your Zapier trigger or action's API configuration. Zapier will then substitute those names for those output field titles in your users' Zaps as in the screenshot above.