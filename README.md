
# winston-transport-sentry-node
[![CircleCI](https://circleci.com/gh/aandrewww/winston-transport-sentry-node.svg?style=svg)](https://circleci.com/gh/aandrewww/winston-transport-sentry-node)
[![node](https://img.shields.io/badge/node-6.4.0+-brightgreen.svg)][node-url]
[![raven](https://img.shields.io/badge/raven-2.x+-brightgreen.svg)][raven-url]
[![winston](https://img.shields.io/badge/winston-3.x+-brightgreen.svg)][winston-url]
[![license](https://img.shields.io/github/license/aandrewww/winston-transport-sentry-node.svg)][license-url]

[@Sentry/node](https://github.com/getsentry/sentry-javascript/tree/master/packages/node) transport for the [winston](https://github.com/winstonjs/winston) v3 logger.

## Index

* [Install](#install)
* [Usage](#usage)
* [Options](#options-options)
  - [Transport related options](#transport-related-options)
  - [Sentry common options](#sentry-common-options)
  - [Info object](#format-info-object)
  - [Log Level Mapping](#log-level-mapping)
* [License](#license)

## Install

```bash
npm install --save winston winston-transport-sentry-node
```


## Usage

You can configure `winston-transport-sentry-node` in two different ways.

With `new winston.Logger`:

```js
const winston = require('winston');
const Sentry = require('winston-transport-sentry-node');

const options = {
  sentry: {
    dsn: 'https://******@sentry.io/12345',
  },
  level: 'info'
};

const logger = new winston.Logger({
  transports: [
    new Sentry(options)
  ]
});
```

Or with winston's `add` method:

```js
const winston = require('winston');
const Sentry = require('winston-transport-sentry-node');

const logger = new winston.Logger();

logger.add(new Sentry(options));
```

See [Options](#options-options) below for custom configuration.

## Options (`options`)

### Transport related options

* `sentry` (Object) - a Sentry configuration object (see [Sentry Common Options](#sentry-common-options))
* `silent` (Boolean) - suppress logging (defaults to `false`)
* `level` (String) - transport's level of messages to log (defaults to `info`)
* `format` (Object) - custom log format (see [Winston Formats](https://github.com/winstonjs/winston#formats))

### Sentry common options

* `dsn` (String) - your Sentry DSN or Data Source Name (defaults to `process.env.SENTRY_DSN`)
* `environment` (String)
* `serverName` (String) - transport's name (defaults to `winston-transport-sentry-node`)
* `debug` (Boolean) - turns debug mode on or off
* `sampleRate` (Number) - sample rate as a percentage of events to be sent in the range of 0.0 to 1.0
* `maxBreadcrumbs` (Number) - total amount of breadcrumbs that should be captured
* ... [Other options](https://docs.sentry.io/error-reporting/configuration/?platform=javascript)

### Format `info` object ([See more](https://github.com/winstonjs/winston#filtering-info-objects))

* `tags` (Object) - tags transforms to extra data for sentry (see [Sentry Extra Context](https://docs.sentry.io/enriching-error-data/context/?platform=javascript#extra-context))


```js
// example

const sentryFormat = format(info => {
  const output = Object.assign({}, info);

  output.tags = {
    path: info.path ? info.path : '',
    request_id: info.label
  };

  return output;
});

new SentryTransport({
  format: sentryFormat(),
  // ...
})
```

### Log Level Mapping

Winston logging levels are mapped by default to Sentry's acceptable levels.

```js
{
  silly: 'debug',
  verbose: 'debug',
  info: 'info',
  debug: 'debug',
  warn: 'warning',
  error: 'error'
}
```

## License

[MIT License][license-url]

[license-url]: LICENSE
[node-url]: https://nodejs.org
[raven-url]: https://github.com/getsentry/raven-node
[winston-url]: https://github.com/winstonjs/winston