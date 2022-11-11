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
- `php artisan inertia:middleware`
- Go to **app/Http/Kernel.php** and add the following to the **web** middleware group...
`    \App\Http\Middleware\HandleInertiaRequests::class,`

## Client Side Inertia Setup
- Check your npm version before proceeding
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
- Go to **web.php** and replace `return view('welcome');` with `return inertia('Welcome');`
- Create **Welcome.vue** in the **Pages** directory
- Populate **Welcome.vue** to test `<h1 class="text-3xl text-pink-700 font-bold">Hello World</h1>`
- `npx mix watch`
- `php artisan serve`

## Inertia Links I
* Some duplication here solved with layout files later
- User the code below to import the Inertia Link component
```
import { Link } from '@inertiajs/inertia-vue3';

export default {
  components: { Link }
};
```
- Create **Shared** directory in **resources/js**
- Create **Nav.vue**
- Add the **Link** importy as above
- Go to a main page you want the nav bar to be shown on and import as below

```
import Nav from "../Shared/Nav"

export default {
  components: { Nav }
}
```
- Use `<Nav />` to show navbar component

## Inertia Links II
- Turning a link into a post request, for a log out route for example, requires adding the method to the link as below
`<Link href="/logout" method="POST" as="button">Log Out</Link>`
- Rendering as a button stops an error when trying to open the post link in a new tab
- Using Tailwind will remove the default button styling automatically

## Inertia Links III
- Bind data to a link using the data property
`<Link href="/logout" method="POST" as="button" :data="{ foo: 'bar' }">Log Out</Link>`
- Get the data on server side as below
`dd(request('foo'));`

## Inertia Links IV - Preserve Scroll
- Add the `preserve-scroll` property to a link to maintain the scroll position

## Inertia Links V - Navlinks & Active Links
- Create a new component in **Shared** called **NavLink.vue**
- Add the home NavLink to your Nav

`<NavLink href="/" :active="$page.component === 'Home'">Home</NavLink>`

- Go to **NavLink.vue** and add your existing Link code

```
<template>
  <Link
    :class="{ 'font-bold underline':active }"
  >
    <slot />
  </Link>
</template>
<script>
  import { Link } from "@inertiajs/inertia-vue3";
  
  export default {
    components: { Link },
    props: {
      active: Boolean
    }
  }
</script>
```

- Go back to the Nav and import the NavLink component

```
import NavLink from "./NavLink";

export default {
  components: { NavLink },
}
```

## Progress Indicators
- Go to resources/js/app.js
- `import { InertiaProgress } from 'inertiajs/progress `
- Add the code below at the bottom of **app.js**, change options as required

```
InertiaProgress.init({
  color: 'red',
  showSpinner: true,
});
```

## Layout Files
- Create **Layout.vue** in the **Shared** directory
- Import the **Nav** in **Layout**

```
import Nav from "./Nav"

export default {
  components: { Nav },
}
```

- Add the content to the Layout template

```
<template>
  <header>
    <h1>App Title</h1>
    <Nav />
  </header>
  <slot />
</template>
```

- Import the layout on your main pages

```
import Layout from "../Shared/Layout";

export default {
  components: { Layout },
}
```

- Wrap everything in your main page templates in the `<Layout>` and `</Layout>` tags
- Remove the <Nav /> on the main pages (your layout file will include it automatically)
