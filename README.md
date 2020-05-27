# hCaptcha Inputfield for ProcessWire

This is an inputfield module for [ProcessWire](https://processwire.com/) that displays an [hCaptcha bot protection](https://www.hcaptcha.com/) and verifies the response. It is primarily built for use with [Form Builder](https://processwire.com/store/form-builder/), but can used programmatically and inserted into any ProcessWire form, including the page edit forms.

## Prerequisites

You need an [hCaptcha account](https://dashboard.hcaptcha.com/signup) to create your API keys. You need two keys:

1. The account-wide **Secret Key** that is used server-side for API requests.
2. A **Site Key** that is used in the hCaptcha widget to differentiate between sites.

You can configure those keys for each individual hCaptcha inputfield, but you can also set them globally in your `config.php`:

```php
// replace with your actual keys
$config->hCaptchaSecretKey = '0x0000000000000000000000000000000000000000';
$config->hCaptchaSiteKey = '10000000-ffff-ffff-ffff-000000000001';
```

## Setup for Form Builder

The most common use case for this module is as bot / spam protection in frontend forms built with [Form Builder](https://processwire.com/store/form-builder/).

1. After you have installed the module, go to *Modules -> Configure -> FormBuilder* and add hCaptcha under *Allowed Input Types*.
2. Now go to edit the Form Builder form you want to add the hCaptcha to (*Setup -> FormBuilder -> Edit -> [your form]*). Add a new field of type hCaptcha.
3. After adding the field, go to the *Details* tab to configure it. The field requires your secret key and a site key, unless you have added them to your `config.php`.
4. The other options mostly reflect the [hCaptcha Container Configuration](https://docs.hcaptcha.com/configuration), so you can change the look and feel and configure JavaScript callbacks. You can also hide the field label above the hCaptcha field.
5. Now fill out the form in the frontend and try submitting it without filling out the captcha. You should see an error message asking you to fill out the captcha and try again. You can change or translate the error messages used by the module through the [ProcessWire's site translation system](https://processwire.com/docs/multi-language-support/).

## API usage

You can create and configure hCaptcha inputfields programmatically:

```php
$hCaptcha = wire('modules')->get('InputfieldHCaptcha');

// the secret key & site key are required unless you have added them to your config.php
$hCaptcha->set('secretkey', '0x0000000000000000000000000000000000000000');
$hCaptcha->set('sitekey', '10000000-ffff-ffff-ffff-000000000001');

// the label is optional
$hCaptcha->set('label', __('Spam Protection'));
// hide the label (default = false)
$hCaptcha->set('hideLabel', 1);
// include the IP address in the API request to hCaptcha? (default = false)
$hCaptcha->set('includeIp', 1);

// change the display theme (default = light)
$hCaptcha->set('theme', 'dark'); // light | dark
// change the display size (default = normal)
$hCaptcha->set('size', 'compact'); // normal | compact | invisible
// set the tabindex (default = 0)
$hCaptcha->set('tabindex', 5); // -1, 0 or any positive integer
// hCaptcha callback (default = empty)
$hCaptcha->set('data-callback', 'yourCustomCallback'); // JavaScript function name
// hCaptcha expired callback (default = empty)
$hCaptcha->set('data-expired-callback', 'yourExpiredCallback'); // JavaScript function name
// hCaptcha error callback (default = empty)
$hCaptcha->set('data-error-callback', 'yourErrorCallback'); // JavaScript function name
```

Use hooks to add the hCaptcha inputfield to specific ProcessWire forms, like the page edit form:

```php
// site/init.php
wire()->addHookAfter('ProcessPageEdit::buildForm', function (HookEvent $event) {
    $form = $event->return;
    $submitButton = $form->getChildByName('submit_save');
    if ($submitButton) {
        $hCaptcha = $event->wire('modules')->get('InputfieldHCaptcha');
        $hCaptcha->set('label', __('Spam Protection'));
        // ... configuration goes here
        $form->insertBefore($hCaptcha, $submitButton);
    }
    $event->return = $form;
});
```

## Script options

The hCaptcha JavaScript library is included only once even if you have multiple hCaptcha inputfields on the same page. It has some configuration options as well, see [hCaptcha Configuration](https://docs.hcaptcha.com/configuration). You can modify those options through a hook:

```php
// site/init.php
wire()->addHookAfter('InputfieldHCaptcha::getScriptOptions', function (HookEvent $e) {
    $e->return = [
        // callback after the hCaptcha script has loaded (default = empty)
        'onload' => 'myCustomCallback',
        // render the widget automatically or manually? (default = onload)
        'render' => 'explicit', // onload | explicit
        // force a specific language for the hCaptcha widget (default = auto-detected browser language)
        'hl' => 'de', // @see https://docs.hcaptcha.com/languages
    ];
});
```
