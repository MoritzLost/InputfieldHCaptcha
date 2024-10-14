# Changelog

## [2.0.1] - 2024-10-14

- **Bugfix:** Fix PHP warnings regarding the usage of `htmlspecialchars`.

## [2.0.0] - 2022-07-18

- **Feature:** Add a permission to bypass hCaptcha completely. See the [documentation](README.md#permissions) for details.
    - If you're upgrading from an earlier version of the module, you may need to add the permission manually. Go to _Access -> Permissions -> Add new_ and add a permission with the name `bypass-hcaptcha`.
- **Breaking change:** After updating, superuser accounts won't see the hCaptcha widget anymore and be allowed to bypass it everywhere. This is a _potential_ breaking change, but mostly relevant to know for development and debugging purposes – make sure to test your forms in a private browser window.

## [1.0.4] - 2021-08-11

- **Bugfix:** Fix a typo in the default error message for a missing captcha response. Make sure to update your translations!
    - **Before**: Captcha reponse is missing. Please fill out the captcha and try again.
    - **After:** Captcha re<ins>s</ins>ponse is missing. Please fill out the captcha and try again.

## [1.0.3] - 2021-02-08

- **Bugfix:** Change method for WireHttp::send for non-cURL environments from 'fopen' to 'auto', since this allows WireHttp to fall back to socket if fopen is not supported.

## [1.0.2] - 2021-02-08

- **Bugfix:** Fix API problems caused by malformed requests. The module now uses cURL on ProcessWire 3.0.167 and fopen with a fallback to socket on older ProcessWire versions, or if cURL isn't supported.

## [1.0.1] - 2020-08-21

- **Bugfix:** Fix errors caused by a missing constant in ProcessWire <3.0.139.

## [1.0.0] - 2020-06-02

- **Milestone:** Initial release!
