# OpenTelemetry Slim Framework auto-instrumentation

**Preferred and simplest way to install auto-instrumentation (c extension plus instrumentation libraries) is to use [opentelemetry-instrumentation-installer](https://github.com/open-telemetry/opentelemetry-php-contrib/tree/main/src/AutoInstrumentationInstaller).**
**The same process can be done manually by installing [c extension](https://github.com/open-telemetry/opentelemetry-php-instrumentation#installation) plus all needed instrumentation libraries like [Slim](#Installation-via-composer)**

## Requirements

* OpenTelemetry extension
* OpenTelemetry SDK and exporters (required to actually export traces)

## Overview
Auto-instrumentation hooks are registered via composer, and spans will automatically be created for:

* `App::handle()` - root span
* `InvocationStrategyInterface` - controller/action
* `RoutingMiddleware::performRouting` - update root span's name with either route name or pattern

To export spans, you will need to create and register a `TracerProvider` early in your application's
lifecycle. This can be done either manually or using SDK autoloading.

### Using SDK autoloading

See https://github.com/open-telemetry/opentelemetry-php#sdk-autoloading

### Manual setup

```php
<?php
require_once 'vendor/autoload.php';

$tracerProvider = /*create tracer provider*/;
$scope = \OpenTelemetry\API\Common\Instrumentation\Configurator::create()
    ->withTracerProvider($tracerProvider)
    ->activate();

//your application runs here

$scope->detach();
$tracerProvider->shutdown();
```

## Installation via composer

```bash
$ composer require open-telemetry/opentelemetry-auto-slim
```