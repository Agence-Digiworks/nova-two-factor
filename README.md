<p align="center">

<img src="https://github.com/Visanduma/nova-two-factor/blob/c26d41cb38c5850e7ee3863e34e5fd3b0c3f18a5/resources/img/nova-two-factor-banner.png?raw=true" />

</p>

[![Latest Version on Packagist](https://img.shields.io/packagist/v/visanduma/nova-two-factor.svg?style=flat-square)](https://packagist.org/packages/visanduma/nova-two-factor)
[![Total Downloads](https://img.shields.io/packagist/dt/visanduma/nova-two-factor.svg?style=flat-square)](https://packagist.org/packages/visanduma/nova-two-factor)

# Nova-Two-Factor
Laravel nova in-dashboard 2FA security feature.


## Install this fork in a laravel project 
In composer.json add to require and repositories sections

```
"require": {
    "visanduma/nova-two-factor": "dev-main"
}

"repositories": {
    "nova-two-factor" : {
        "type": "vcs",
        "url": "https://github.com/Agence-Digiworks/nova-two-factor.git"
    }
}    
```

Then run 
```
composer update
```

## Versions 
Use v3.* for Laravel Nova 5

Use v2.* for Laravel Nova 4

Use ^v1.* for Laravel Nova 3


```
\\ Nova 5

composer require visanduma/nova-two-factor:~3

\\ Nova 4

composer require visanduma/nova-two-factor:~2


\\ Nova 3

composer require visanduma/nova-two-factor:~1

```


## What's New

### v3.0
- Nova 5.0 support
  
### v2.2.14
- Fixed offline QRCode display issue

### v2.2.3
- Fixed foreign key issue (need to run migration)
- Translation fixes

### v2.2.2
- Clear option for current Two  FA settings
### v2.2.0
- Reauthorize any routes using *2FA Prompt* dialog.



## Interface

Setup 2FA

![screenshot](/resources/img/sc-1.png)

Enable/Disable feature

![screenshot](/resources/img/sc-2.png)

Nova login screen with 2FA security

![screenshot](/resources/img/sc-3.png)

Reauthorize any route using 2FA prompt

![screenshot](/resources/img/sc-4.png)

Install the package

`` composer require visanduma/nova-two-factor ``


1. Pubish config & migration

`` php artisan vendor:publish --provider="Visanduma\NovaTwoFactor\ToolServiceProvider" ``


Change configs as your needs

``` 

return [
    // defaults to app.name when null
    'display_name' => null,
    
     // enable or disable 2FA feature. default is enabled
    'enabled' => env('NOVA_TWO_FA_ENABLE',true),
    
    // name of authenticatable entity table. usually - users
    'user_table' => 'users',
    
    // Entity primary key
    'user_id_column' => 'id',
    
    // authenticatable model class
    'user_model' => \App\Models\User::class

    /* Change visibility of Nova Two Fa menu in right sidebar */
    'showin_sidebar' => true,

    'menu_text' => 'Two FA',

    'menu_icon' => 'lock-closed',

    /* Exclude any routes from 2fa security */
    'except_routes' => [
        //
    ],

    /**
     * reauthorize these urls before access, withing given timeout
     * you are allowed to use wildcards pattern for url matching
     **/

    'reauthorize_urls' => [
       // 'resources/users/new',
       // 'resources/users/*/edit',
    ],

    /* timeout in minutes */

    'reauthorize_timeout' => 5,


    /* QR code can be generate using  online API or inbuilt 'BaconQrCode' package*/

    'use_offline_qr' => false,

];

```


2. Use ProtectWith2FA trait in configured model

``` 
<?php

namespace App\Models;

use Visanduma\NovaTwoFactor\ProtectWith2FA;

class User extends Authenticatable{

    use ProtectWith2FA;
}

```



3. Add TwoFa middleware to nova config file


``` 
/*
    |--------------------------------------------------------------------------
    | Nova Route Middleware
    |--------------------------------------------------------------------------
    |
    | These middleware will be assigned to every Nova route, giving you the
    | chance to add your own middleware to this stack or override any of
    | the existing middleware. Or, you can just stick with this stack.
    |
    */

    'middleware' => [
        ...
        \Visanduma\NovaTwoFactor\Http\Middleware\TwoFa::class
    ],

```


4. Register NovaTwoFactor tool in Nova Service Provider

``` 
<?php

class NovaServiceProvider extends NovaApplicationServiceProvider{

public function tools()
    {
        return [
            ...
            new \Visanduma\NovaTwoFactor\NovaTwoFactor()

        ];
    }

}


```

5. Run `` php artisan migrate ``
6. You are done !
