# hCaptcha Inputfield for ProcessWire

- description, gateway to hCaptcha, field error if not successfully filled

## Prerequisites

- create account, get secret key, create site + get site key
- (optional) configure secret key & site key through config.php

## Setup for Form Builder

- most common use case
- Form Builder configuration -> allow hCaptcha inputfield
- forms -> edit form -> add field hCaptcha
- details tab -> configuration (only site & secret key required if not set through the config)

## Add to page edit form

```php
// site/init.php
wire()->addHookAfter('ProcessPageEdit::buildForm', function (HookEvent $event) {
    $form = $event->return;

    $submit = $form->getChildByName('submit_save');
    if ($submit) {
        $hCaptcha = $event->wire('modules')->get('InputfieldHCaptcha');

        // the label is optional
        $hCaptcha->set('label', __('Spam Protection'));

        // the secret key & site key can also be set in the config.php
        $hCaptcha->set('secretkey', '0x0000000000000000000000000000000000000000');
        $hCaptcha->set('sitekey', '10000000-ffff-ffff-ffff-000000000001');

        $form->insertBefore($hCaptcha, $submit);
    }

    $event->return = $form;
});
```

## Configuration

- Theme & callback options, see https://docs.hcaptcha.com/configuration
- hide field label optional
- include IP optional (for hCaptcha API)

## Script options

- modify through hook (in init.php), see https://docs.hcaptcha.com/configuration

```php
// site/init.php
wire()->addHookAfter('InputfieldHCaptcha::getScriptOptions', function (HookEvent $e) {
    $e->return = [
        'onload' => 'myCustomCallback',
        'render' => 'explicit',
        'hl' => 'de',
    ];
});
```
