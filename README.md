# Laravel Inertia Quickstart
## Create new Laravel project
- `Laravel new demo`
- cd demo
## Server Side Inertia Setup
- `composer require inertiajs/inertia-laravel`
- rename welcome.blade.php to app.blade.php
- replace the contents of app.blade.php with the following...

`<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
    <link href="{{ mix('/css/app.css') }}" rel="stylesheet" />
    <script src="{{ mix('/js/app.js') }}" defer></script>
    @inertiaHead
  </head>
  <body>
    @inertia
  </body>
</html>`
