phpParticle
========

PHP Class for interacting with the Particle Cloud (particle.io)

## TODO

Work in progress, but main tasks are:

* [ ] Create tests.
* [x] Remove old Spark code.
* [x] Replace examples.
* [ ] Maybe remove the debug logging, given we have enough injected points to set debug.

## Quick Dev Installation

This is a development only package at the moment.

First check out a clone:

    git clone git@github.com:academe/Particle.git

In the same directory, create composer.json

~~~json
{
    "require": {
        "psr/log": "^1.0",
        "psr/http-message": "^1.0",
        "guzzlehttp/psr7": "^1.2",
        "guzzlehttp/guzzle": "^6.1"
    },
    "autoload": {
        "psr-4": {
            "academe\\particle\\": "Particle/src"
        }
    }
}
~~~

Then pull in the dependencies:

    composer.phar update

index.php demo:

~~~php
<?php

require "vendor/autoload.php";

use Academe\Particle\ParticleApi;
use Academe\Particle\Log\EchoLogger;
use Academe\Particle\Psr7\GuzzleConnnector;

$logger = new EchoLogger('HTML');

$device = 'device-id';
$access_token = 'access-token';
$cloud_email = 'cloud-email';
$cloud_password = 'cloud-password';

$particle = new ParticleApi(new GuzzleConnnector());

$particle = $particle
    ->setLogger($logger) // optional
    ->setAccessToken($access_token)
    ->setDebug(false) // or true
    ->setAuth($cloud_email, $cloud_password);

// Try any of these messages

//$request = $particle->callFunction($device, 'function', 'function-parameters', true);
//$request = $particle->listDevices();
//$request = $particle->getDevice($device);
//$request = $particle->renameDevice($device, 'a-new-name');
//$request = $particle->getVariable($device, 'variable-name', true);
//$request = $particle->listAccessTokens();
//$request = $particle->newAccessToken();
//$request = $particle->deleteAccessToken('token-id');
//$request = $particle->claimDevice($device, true);
//$request = $particle->listWebhooks();
//$request = $particle->newWebhook('event', 'http://example.com/', []);
//$request = $particle->deleteWebhook('webhook-id');
//$request = $particle->signalDevice($device);
//$request = $particle->uploadFirmware($device, 'tinker.cpp', 'phpParticle/examples/tinker.cpp', false);

// Send the message.
$client = new \GuzzleHttp\Client(['defaults' => ['timeout' => 2]]);
$response = $client->send($request);

// See the result
echo "<pre>";
echo "Status=" . $response->getStatusCode() . "\n";
echo "Reason=" . $response->getReasonPhrase() . "\n";
var_dump(json_decode($response->getBody()));
~~~

## Example Class

Examples have been put into the `Academe\Particle\Example` class. It can be run like this:

~~~php
use Academe\Particle\Example;

$accessToken = 'your-account-access-token';
$deviceId = 'your-device-id';
$email = 'your-cloud-email-username';
$password = 'your-cloud-password';

// Instantiate the example class.
$example = new Example($access_token, $device, $email, $password);

// Flash the device with the Tinker application.
$response = $example->flashTinker();
//$response = $example->setDeviceName();
//$response = $example->setDeviceName('christmas_turkey');
//$response = $example->listDevices();
//$response = $example->getDevice();
//$response = $example->getDevice('20033307343c03805403a138');
//$response = $example->newAccessToken();
//$response = $example->listTokens();
//$response = $example->deleteAccessToken('176a67f0d31647beac429252af8663a5040a945c');
//$response = $example->callFunction();
//$response = $example->listOranizations();
//$response = $example->removeMember('my_organization', 'someone@example.com');

echo "Status=" . $response->getStatusCode() . "\n";
echo "Reason=" . $response->getReasonPhrase() . "\n";
echo "Detail=" . $response->getBody() . "\n";
~~~

Don't forget to install Guzzle to run these examples:

    composer require guzzlehttp/guzzle

Most of the other examples in this class require the Tinker application to be installed.

## Implemented Features

### Device Management
- List Devices
- Get device info 
- Rename/Set device name
- Call Particle Function on a device
- Grab the value of a Particle Variable from a device
- Remote (Over the Air) Firmware Uploads
- Device signaling (make it flash a rainbow of colors)

### Access Token Management
- Generate a new access token
- List your access tokens
- Delete an access token

### Webhook Management

- List Webhooks
- Add Webhook
- Delete Webhook

### Account/Cloud Management
- Use a local particle cloud
- Claim core or photon
- Remove core or photon

## Not Yet Implemented Features
- OAuth Client Creation (/v1/clients)
- Advanced OAuth topics
