---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - shell

toc_footers:
  - <a href='https://notibot.kihtrak.com'>Open Web App</a>
  - <a href='https://notibot.kihtrak.com'>Download iOS App</a>
  - <a href='https://notibot.kihtrak.com'>Download Android App</a>

search: true

code_clipboard: true
---

# Introduction

Welcome to the NotiBot API! You can use our API to programmatically send you or your team notifications.

Please download the app for the best possible experience. It is available for both [iOS](https://notibot.kihtrak.com) and [android](https://notibot.kihtrak.com).

# Set-Up

Using either the app or [web app](https://notibot.kihtrak.com), create a new project. 

Give it a good name as it will be used as a sort of "pseudo API key"

# Usage

## Super Basic
> To send a notification:

```shell
curl https://n.kihtrak.com/proj=<PROJECT NAME>
```

```javascript
fetch("https://n.kihtrak.com/proj=<PROJECT NAME>")
// Note: If you are using this code on a website, 
// make sure the site's CORS headers will allow the
// request to go through.
```

> It's that simple!

With the project name you choose in the previous step, you can make your first API call.

<aside class="notice">
proj is the only required URL parameter. Without any other parameters you will get a empty notification with only your project name as the title.
</aside>


## Send a Custom Notification

> To send a notification:

```shell
curl https://n.kihtrak.com/proj=<PROJECT NAME>\
&title=<OPTIONAL NOTIFICATION TITLE>\
&body=<OPTIONAL NOTIFICATION BODY>\
&webhook=<OPTIONAL WEBHOOK>\
&webhookParam=<true/false>
```

```javascript
fetch("https://n.kihtrak.com/proj=<PROJECT NAME>\
&title=<OPTIONAL NOTIFICATION TITLE>\
&body=<OPTIONAL NOTIFICATION BODY>\
&webhook=<OPTIONAL WEBHOOK>\
&webhookParam=<true/false>")
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

### HTTP Request

`GET https://n.kihtrak.com/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
proj | required | The name of the project this notification should be sent to.
title | *proj* | The title that appears on the notification.
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
	nonExistant.prop
}catch(e){
  fetch(`https://n.kihtrak.com/proj=<PROJECT NAME>\
    &title=Error!&body=${e.toString()}`)
}
```

> This will send a notification if an error occurs

Error catching and logging is only useful if someone is checking those logs. Many edge case bugs may be missed because this arise much later in a project's life span. 

With NotiBot, you will be instantly notified if an error does arise later on.

