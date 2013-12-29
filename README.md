# Yii ReturnUrl

Maintain the state of Return Urls by using the request.

[![Mr PHP](https://raw.github.com/cornernote/mrphp-assets/master/img/code-banner.png)](http://mrphp.com.au) [![Project Stats](https://www.ohloh.net/p/yii-return-url/widgets/project_thin_badge.gif)](https://www.ohloh.net/p/yii-return-url) 

[![Latest Stable Version](https://poser.pugx.org/cornernote/yii-return-url/v/stable.png)](https://packagist.org/packages/cornernote/yii-return-url) [![Build Status](https://travis-ci.org/cornernote/yii-return-url.png?branch=master)](https://travis-ci.org/cornernote/yii-return-url)


You might be saying, Yii already handles a returnUrl perfectly fine with the `CWebUser::getReturnUrl()` and `CWebUser::setReturnUrl()` methods.  Why not use those?

These methods store the returnUrl into a single variable in the users session.  This becomes a flaw when we have multiple tabs open.  Take the following scenario:

- A user navigates to a page that sets a returnUrl.  The page is for a form they have to complete.
- The phone rings, and they are required to fill in a different form.  They achieve this by opening another tab.
- As they navigate to the new page, their old returnUrl is overwritten by the new one, they complete the second form and everything seems normal.
- They then return to their first form, and after submission they get taken to the second returnUrl and their navigation path appears broken.

The solution is to pass the returnUrl into the GET and POST request by embedding it into your links and forms.  This extension makes it very easy to do and solves many common problems including the maximum length of a GET request.


### Contents

[Features](#features)  
[Installation](#installation)  
[Configuration](#configuration)  
[Usage](#usage)  
[License](#license)  
[Links](#links) 


## Features

- Allows a URL to be consistant with the page the user is viewing, even if they open other tabs.
- Easily embed return URLs into your links or forms.
- Handles very long returnUrl values by passing a key in the GET params.


## Installation

Please download using ONE of the following methods:


### Composer Installation

```
curl http://getcomposer.org/installer | php
php composer.phar require cornernote/yii-return-url
```


### Manual Installation

Download the [latest version](https://github.com/cornernote/yii-return-url/archive/master.zip) and move the `yii-return-url` folder into your `protected/extensions` folder.


## Configuration

Add the path to yii-return-url to the `components` in your yii configuration:

```php
return array(
	'components' => array(
	        'returnUrl' => array(
			'class' => 'vendor.cornernote.yii-return-url.components.EReturnUrl',
			// if you downloaded into ext
			//'class' => 'ext.yii-return-url.components.EReturnUrl',
	        ),
	),
);
```


## Usage

Your user is on a search results page, and you have a link to an update form.  After filling in the form you want the user to be returned to the page they started from.

On the start page, add a returnUrl to your link, for example in `views/post/index.php`:
```php
// generate a returnUrl link value
// pass true so that it points to the current page
$returnUrlLinkValue = Yii::app()->returnUrl->getLinkValue(true);
CHtml::link('edit post', array('post/update', 'id' => $post->id, 'returnUrl' => $returnUrlLinkValue));
```

On the update page, add a returnUrl to your form, for example in `views/post/update.php`:
```php
// generate a returnUrl form value
// pass false so that it points to the returnUrl from the request params provided by your link
$returnUrlFormValue = Yii::app()->returnUrl->getFormValue(false);
CHtml::hiddenField('returnUrl', $returnUrlFormValue);
```

In the controller action that handles the form, change the call to `$this->redirect()`, for example in `Post::actionUpdate()`
```php
$altUrl = array('post/index'); // this is where we used to redirect to, we use it as a failback
$this->redirect(Yii::app()->returnUrl->getUrl($altUrl));
```


## License

- Author: Brett O'Donnell <cornernote@gmail.com>
- Author: Zain Ul abidin <zainengineer@gmail.com>
- Source Code: https://github.com/cornernote/yii-return-url
- Copyright © 2013 Mr PHP <info@mrphp.com.au>
- License: BSD-3-Clause https://raw.github.com/cornernote/yii-return-url/master/LICENSE


## Links

- [Yii Extension](http://www.yiiframework.com/extension/return-url)
- [Composer Package](https://packagist.org/packages/cornernote/yii-return-url)
- [MrPHP](http://mrphp.com.au)
