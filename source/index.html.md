---
title: NotiBot API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - shell

toc_footers:
  - <a href='https://notibot.kihtrak.com'>Open Web App</a>
  - <a href='https://apps.apple.com/us/app/notibot/id1568618783?mt=8'>Download iOS App</a>
  - <a href='https://play.google.com/store/apps/details?id=com.kihtrak.NOTIBOT'>Download Android App</a>

search: true

code_clipboard: true
---

# Introduction

Welcome to the NotiBot API! You can use our API to programmatically send you or your team notifications.

Please download the app for the best possible experience. It is available for both [iOS](https://apps.apple.com/us/app/notibot/id1568618783?mt=8) and [android](https://play.google.com/store/apps/details?id=com.kihtrak.NOTIBOT).

# Set-Up

Using either the app or [web app](https://notibot.kihtrak.com), create a new project. 

Give it a good name as it will be used as a sort of "pseudo API key"

# Usage

## Super Basic
> To send a notification:

```shell
curl https://n.kihtrak.com/?project=<PROJECT NAME>
```

```javascript
fetch("https://n.kihtrak.com/?project=<PROJECT NAME>")
```

> It's that simple!

With the project name you choose during set up, you can make your first API call.

You should get something like this:

![notification with no custom title or body](ProjectNameNotification.png)

<aside class="notice">
<em>project</em> is the only required URL parameter. Without any other parameters you will get a empty notification.
</aside>

## Send a Custom Notification

> To send a notification:

```shell
curl https://n.kihtrak.com/?project=<PROJECT NAME>&title=<OPTIONAL NOTIFICATION TITLE>&body=<OPTIONAL NOTIFICATION BODY>&webhook=<OPTIONAL WEBHOOK>&webhookParam=<true/false>
```

```javascript
fetch("https://n.kihtrak.com/?project=<PROJECT NAME>&title=<OPTIONAL NOTIFICATION TITLE>&body=<OPTIONAL NOTIFICATION BODY>&webhook=<OPTIONAL WEBHOOK>&webhookParam=<true/false>")
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "error": null, // A string if there is an error
    "notificationsSent": 1,
    "notificationsSentSuccessfully": 1,
    "nonCriticalErrors": [
        "Errors are very common when sending notifications, anything appearing here can be safely ignored",
    ]
}
```

### HTTP Request

`GET https://n.kihtrak.com/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
project | required | The name of the project this notification should be sent to.
key | null | The API key for the project. API keys are disabled by default and must be enabled in project options.
title | *project* | The title that appears on the notification.
body | null | The body of the notification (the text under the title).
webhook | null | A URL the webhook will go to. If this parameter isn't left empty the recipient of the notification will be provided with an option to make a call to the provided link. This can be useful if there are certain actions you would like to trigger remotely after receiving a notification.
webhookParam | false | If true, the recipient will be able to type out a string that will be appended to the webhook URL before it is called.

# Use Cases

## Error Logging

> Example: 

``` shell
# example is in the javascript tab
```

``` javascript
try {
  // Example of code with error:
  nonExistant.prop
}catch(e){
  fetch(`https://n.kihtrak.com/?project=<PROJECT NAME>&title=Error!&body=${e.toString()}`)
}
```

> This will send a notification if an error occurs in the try block

Error catching and logging is only useful if someone is checking those logs. Many edge case bugs may be missed because this arise much later in a project's life span. 

With NotiBot, you will be instantly notified if an error does arise later on.

![notification with error message](ErrorNotification.png)

## Get Notified of Visitors or Milestones

You can use NotiBot for easy growth monitoring. 

### Know exactly when to pop open that bottle of champagne 🍾! 

``` shell
# example is in the javascript tab
```

``` javascript
// let totalUsers be some kind of persistent storage variable
// assuming you are using something like express:
app.post('/createUser', (req, res) => {
  // handle account creation...
  totalUsers++
  if(totalUsers%100 == 0){
    fetch(`https://n.kihtrak.com/?project=<PROJECT NAME>&title=You hit ${totalUsers} total users!&body=🎉🎈🥳`)
  }
})
```

>

![notification celebrating an app milestone](TotalUsersNotification.png)

### Or know whether that friend who swore they would download your app actually installed it 😉.

``` shell
# example is in the javascript tab
```

``` javascript
// assuming you are using something like express:
app.post('/createUser', (req, res) => {
  // handle account creation...
  const { name, isPremium } = req.query
  fetch(`https://n.kihtrak.com/?project=<PROJECT NAME>&title=${name} created an account!&body=${isPremium?`They have signed up for Premium!`:`They signed up for the free plan`}`)
})
```

>

![notification informing the reader that a new user has signed up](UserSignedUpNotification.png)

## Automation

NotiBot is built first and foremost for developers, so you can integrate it into your work, hobbies or daily life in anyway your heart desires. 

Especially when automating tasks, it can be useful to be able to get reliable and timely information. NotiBot makes keeping yourself in the loop easy.

### Know when a task finishes: 

``` shell
# example is in the javascript tab
```

``` javascript
makeMeSomeHotCoco()
.then(()=>fetch(`https://n.kihtrak.com/?project=<PROJECT NAME>&title=Hot coco finished!&body=Time to drink up!\n☕☕☕`))
```
>

![notification informing the user that their coco is ready to drink](HotCocoFinishedNotification.png)

### Start a task: 

``` shell
# example is in the javascript tab
```

``` javascript
// assuming you are using something like express:
app.post('/makeCoco', (req, res) => {
  // Code to make Coco...
})
cron.schedule('0 8 * * *', () => { // Run every day at 8am
  fetch(`https://n.kihtrak.com/?project=<PROJECT NAME>&title=Want some hot coco?&body=Respond to this notification with the webhook trigger to start making the coco 👇&webhook=<server address>/makeCoco`)
});
```
> You will get a prompt for hot coco every day at 8am. The notification will also include a quick action that will make a call to whatever webhook you specified.

![notification allowing the user to trigger their coco machine remotely](WantHotCocoNotification.png)
![actions allowing the user to trigger their coco machine remotely](WantHotCocoActions.png)