# Changelog

## [1.0.3] - 2021-02-08

- **Bugfix:** Change method for WireHttp::send for non-cURL environments from 'fopen' to 'auto', since this allows WireHttp to fall back to socket if fopen is not supported.

## [1.0.2] - 2021-02-08

- **Bugfix:** Fix API problems caused by malformed requests. The module now uses cURL on ProcessWire 3.0.167 and fopen with a fallback to socket on older ProcessWire versions, or if cURL isn't supported.

## [1.0.1] - 2020-08-21

- **Bugfix:** Fix errors caused by a missing constant in ProcessWire <3.0.139.

## [1.0.0] - 2020-06-02

- **Milestone:** Initial release!
