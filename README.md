# Laravel Inertia Tailwind Quickstart 
## Create new Laravel project
- `Laravel new demo`
- cd demo
## Server Side Inertia Setup
- `composer require inertiajs/inertia-laravel`
- rename **welcome.blade.php** to **app.blade.php**
- replace the contents of **app.blade.php** with the following...
```
<!DOCTYPE html>
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
</html>
```
- `php artisan inertia:middelware`
- Go to **app/Http/Kernel.php** and add the following to the **web** middleware group...
`HandleInertiaRequests::class`

## Client Side Inertia Setup
- `npm install @inertiajs/inertia @inertiajs/inertia-vue3`
- `npm install vue@next`
- Go to **resources/js**
- Delete **bootstrap.js**
- Replace the contents of **app.js** with the following...
```
import { createApp, h } from 'vue'
import { createInertiaApp } from '@inertiajs/inertia-vue3'

createInertiaApp({
  resolve: name => require(`./Pages/${name}`),
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el)
  },
})
```
- Create the **Pages** directory in **/resources/js**
- Go to **webpack.mix.js** and add the following below mix.js
```
.vue(3)
.version();
```
- `npm install`
- Run `npx mix` **twice**

## Install Tailwind
- `npm install -D tailwindcss postcss autoprefixer`
- `npx tailwindcss init -p`
- Configure the template paths in **tailwind.config.js** as below...
```
content: [
    "./resources/**/*.blade.php",
    "./resources/**/*.js",
    "./resources/**/*.vue",
  ],
```
- Add the Tailwind directives in **'./resources/css/app.css'** as below...
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Create first page
- Go to **web.php** and replace `return view('welcome');` with `return inertia('welcome');`
- Create **welcome.vue** in the **Pages** directory
- Populate **welcome.vue** to test `<h1 class="text-3xl font-bold">Hello World</h1>`
- `npx mix watch`
- `php artisan serve`
