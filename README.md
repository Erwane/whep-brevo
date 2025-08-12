# [Brevo](https://www.brevo.com/) (SendInBlue) webhook handler for [WHEP](https://github.com/Erwane/whep-brevo) project

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
[![codecov](https://codecov.io/gh/Erwane/whep-brevo/branch/2.0/graph/badge.svg?token=YCGYJQYBXR)](https://codecov.io/gh/Erwane/whep-brevo)
[![Build Status](https://github.com/Erwane/whep-brevo/actions/workflows/ci.yml/badge.svg?branch=2.0)](https://github.com/Erwane/whep-brevo/actions)
[![Packagist Downloads](https://img.shields.io/packagist/dt/Erwane/whep-brevo)](https://packagist.org/packages/Erwane/whep-brevo)
[![Packagist Version](https://img.shields.io/packagist/v/Erwane/whep-brevo)](https://packagist.org/packages/Erwane/whep-brevo)

Webhook handler for [Brevo](https://www.brevo.com/) (SendInBlue) emailing provider.

## Usage

```shell
composer require erwane/whep-brevo
```

```php
use WHEP\Exception\SecurityException;  
use WHEP\Exception\WHEPException;  
use WHEP\Factory;  

try {
    $provider = Factory::provider('brevo', [
        'client_ip' => $_SERVER['REMOTE_ADDR'] ?? null, // Use method from your framework to get the ServerRequest client ip.
        'callbacks' => [
            ProviderInterface::EVENT_BLOCKED => [$this, 'callbackInvalidate'],
            ProviderInterface::EVENT_BOUNCE_QUOTA => [$this, 'callbackUnsub'],
        ],
    ]);

    // process the data.
    $provider->process($webhookData);
    
    // Data available from provider getters.
    $recipient = $provider->getRecipient();
    
    // Launch callbacks
    $provider->callback();
} catch (SecurityException $e) {
    // log ?
} catch (WHEPException $e) {
    // log ?
}
```

See [WHEP Client README](https://github.com/Erwane/whep-client) for options, events and getters.
