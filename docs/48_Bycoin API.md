---
id: docs_48
title: Bycoin API
sidebar_label: Bycoin API
---

bycoin api调用仅在bycoin dapp浏览器中支持。

##### 检查 bycoin

你可以通过!window.bycoin检查bycoin浏览器，如果当前的浏览器是bycoin bapp浏览器返回true

##### callAPI

我们提供了一系列的方法来提高您的dapp的性能。每个方法都有一个apiname。使用如下：

`bycoin.callAPI(apiName, options, callback)`

| Params | Type | Description |
| --- | --- | --- |
| apiName | String | name of the method which you want to invoke, you will see the available apiNames below |
| options | Object | options depend on the specific method |
| callback | Function | callback of method |


```javascript
bycoin.callAPI(apiName, params, (error, result) => {
  if(error) {
    alert(error.message)
  } else {
    alert(result)
  }
})
```

#### API

##### navigator

##### navigator.getOrientation

获取当前APP的状态定向，返回'LANDSCAPE'或者'PORTRAIT'

```javascript
bycoin.callAPI('navigator.getOrientation', function(err, result) {
  console.log(result)
})
```

##### navigator.setOrientation

设置应用程序的定向，参数是'landscape'或者'portrait'

```javascript
bycoin.callAPI('navigator.setOrientation', 'landscape')
```

##### navigator.closeDapp

关闭当前Bapp的窗口

```javascript
bycoin.callAPI('navigator.closeDapp')
```

##### navigator.goBack

定向到当前网页的上一个历史记录，如：history.go(-1). 如果没有以前的记录，该方法将关闭当前的dapp窗口。

```javascript
bycoin.callAPI('navigator.goBack')
```

#### native

##### native.alert

显示本机警报对话框以显示消息，如window.alert。

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| message | String | true | null | message to show |


```javascript
bycoin.callAPI('native.alert', 'winner winner, chicken dinner！')
```

##### native.confirm

显示本机确认对话框，如window.confirm。如果用户单击了“取消”按钮，则回调函数将返回一个错误。

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| title | String | true | null | dialog title |
| message | String | true | null | dialog content |
| cancelText | String | true | null | cancel button text |
| confirmText | String | true | null | confirm button text |


```javascript
bycoin.callAPI('native.confirm', {
  title: 'quick question',
  message: 'is Javascript the worst language?',
  cancelText: 'no',
  confirmText: 'yes',
}, function(err, result) {
  if(err) {
    console.log('no')
  } else {
    console.log('yes')
  }
})
```

##### native.toastInfo

异步消息通知用户

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| text | String | true | null | text to toast |


```javascript
bycoin.callAPI('native.toastInfo', '123456789')
```

##### native.saveImage

将图像保存到本地磁盘

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| url | String | true | null | image url |


```javascript
bycoin.callAPI('native.saveImage', { url:"https://photo.16pic.com/00/53/34/16pic_5334613_b.jpg" },function (err, ret) {
     if(err) {
    alert(err)
  } else {
    alert('save image success.')
  }
  })
```

##### native.saveScreenshot

将当前屏幕快照保存到本地磁盘

```javascript
bycoin.callAPI('native.saveScreenshot', function (err, ret) {
    if(err) {
        alert(err)
      } else {
        alert('save sreenshot success.')
      }
  })
```

##### native.selectPicture

从相册选择图片或拍照。它将返回base64格式的图像数据（jpeg默认值）。

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| maxWidth | Number | false | null | response will be corpped |
| maxHeight | Number | false | null | response will be corpped |
| quality | Number | false | 1 | quality of image, range of value is 0 to 1 |


相应:

| property | Type | Description |
| --- | --- | --- |
| data | String | base64 data, default prefix 'data:image/jpeg;base64,' |
| width | Number | image width |
| height | Number | image height |
| fileSize | Number | file size (b) |


```javascript
bycoin.callAPI('native.selectPicture',{
  maxWidth: 400,
  maxHeight: 200
}, function (err, ret) {
  if(err) {
    alert(err.message)
  } else {
    document.getElementById('imgContainer').src = ret.data
  }
})
```

##### native.setClipboard

设置文本到用户的剪贴板。

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| text | String | true | null | text to set |


```javascript
bycoin.callAPI('native.setClipboard', 'are you ok?')
```

##### native.share

打开本机共享组件

| Params | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| title | String | true | null | title |
| message | String | true | null | content |
| url | String | true | null | link. If you want to share an image, set url to base64 data |


```javascript
bycoin.callAPI('native.share', {
    title: 'dapp example',
    message: 'this is example of dapp sdk',
    url: location.href,
  }, function (err, ret) {
    if(err) {
      alert(err.message)
    } else {
      alert('share success!')
    }
  })
```

##### native.scanQRCode

打开扫一扫，扫描二维码，返回二维码的字符串内容

```javascript
bycoin.callAPI('native.scanQRCode', function (err, text) {
    if(err) {
      alert(err.message)
    } else {
      alert(text)
    }
  })
```

#### device

##### device.getCurrentLanguage

获取用户的当前语言设置。如果您的Bapp应该支持多种语言，则此信息可能很有用，但我们已将locale参数添加到Bapp的url中。在大多数情况下，您不需要调用此api

Available locale:

- zh-CN
- en-US

```javascript
bycoin.callAPI('device.getCurrentLanguage', function(err, language) {
  if(err) {
    alert(err)
  } else {
    alert(language)
  }
})
```

##### device.getCurrentCurrency 

获取用户的当前货币设置

Available currencies:

- CNY
- USD
- BTC

```javascript
bycoin.callAPI('device.getCurrentCurrency', function(err, currency) {
  if(err) {
    alert(err)
  } else {
    alert(currency)
  }
})
```

