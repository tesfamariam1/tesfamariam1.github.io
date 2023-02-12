---
title: Laravel + Vue Full-stack Application
date: 2023-01-30 +03:00
categories: [Laravel, Vue, Vite]
tags: [laravel,vue,full-stack,vite]
---

# How To Install Vue 3 in Laravel 9 with Vite
Use the following steps to install Vue 3 in the laravel 9 application.

- Install laravel 9 App
- Install NPM Dependencies
- Install Vue 3
- Update vite.config.js
- Compile the assets
- Create Vue 3 App
- Create Vue 3 Component
- Connect Vue 3 Component with Laravel blade file and use vite directive to add assets.
- Update Laravel Routes
- Start The Local Server

## 1.  Install laravel 9 App
First, open Terminal and run the following command to create a fresh laravel project:

```composer create-project --prefer-dist laravel/laravel:^9.0 laravel9-vue3-vite```

or, if you have installed the Laravel Installer as a global composer dependency:

```laravel new laravel9-vue3-vite```

## 2. Install NPM Dependencies
Run the following command to install frontend dependencies:

```npm install```
## 3. Install Vue 3
Now after installing node modules we need to install vue 3 in our application, for that execute the following command in the terminal npm install vue@next vue-loader@next. vue-loader is a loader for webpack that allows you to author Vue components in a format called Single-File Components. vue-loader@next is a loader that is for webpack to author Vue components in single-file components called SFCs.

```npm install vue@next vue-loader@next```

## 4. Install vitejs/plugin-vue plugin
In laravel 9 latest release install vitejs/plugin-vue plugin for installing vue3 or vue in laravel. This plugin provides required dependencies to run the vuejs application on vite. Vite is a  build command that bundles your code with Rollup and runs of localhost:3000 port to give hot refresh feature.

```npm i @vitejs/plugin-vue```

## 5. Update vite.config.js file
Vite is a module bundler for modern JavaScript applications. Open vite.config.js and copy-paste the following code. First invoice defineConfig from vite at the top of the file and also import laravel-vite-plugin. Here plugins() take the path of the js and CSS file and create bundles for your application. you need to add vue() in the plugins array.

```js
// vite.config.js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue'


export default defineConfig({
    plugins: [
        vue(),
        laravel([
            'resources/css/app.css',
            'resources/js/app.js',
        ]),
    ],
});
```

## 6. Vite Dev Server Start
Now after installing the vue 3, we need to start the dev server for vite for that run the following command and it will watch your resources/js/app.js file and resources/css/app.css file. It also starts a vite server on http://localhost:3000. you can not open it in the browser as it is for vite hot reload and it runs in the background and watches the assets of your application like js and CSS.

```npm run dev```
## 7. Create Vue 3 App
In resources/js/app.js create an instance of the vue 3 firstly you import { createApp } from 'vue' and createApp It takes a parameter here we have passed App. Before that, you can create a vue file which is the main file responsible for the vuejs content name is App.vue.
```js
// app.js
require('./bootstrap');

import {createApp} from 'vue'

import App from './App.vue'

createApp(App).mount("#app")
```
## 8. Create Vue 3 Component
Under the js folder create a file name 'App.vue' and write content for this example let’s write simple "How To Install Vue 3 in Laravel 9 with Vite - TechvBlogs" you can change it according to your requirement.

```js
<template>
    How To Install Vue 3 in Laravel 9 with Vite - TechvBlogs
</template>
```
## 9. Connect Vue 3 Component with Laravel blade file
In this step, go-to resource / views  directory, create the  app.blade.php  file, and add the following code to app.blade.php  as follow:
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>How To Install Vue 3 in Laravel 9 with Vite</title>

	@vite('resources/css/app.css')
</head>
<body>
	<div id="app"></div>

	@vite('resources/js/app.js')
</body>
</html>
```
## 10. Update Laravel Routes
Open routes/web.php and replace welcome.blade.php with vue.blade.php file to load the vue.blade.php file where our vuejs code will execute.
```php
<?php

use Illuminate\Support\Facades\Route;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('app');
});
```
## 11. Update .env file
Open .env file and update APP_URL and set APP_URL=http://localhost:8000. It will help vite to check the js and CSS updates and changes them in the browser instantly.
```php
APP_URL=http://localhost:8000
```
## 12. Start the Local server
Now open a new terminal and execute the following command from your terminal to run the development server. It runs the project on localhost 8000 port by default but you can change it also. Run npm run dev server also so that site will watch the changes in the vuejs templates and will update automatically to the browser. if you are running another project on the same port number.
```
php artisan serve
```

and navigate to the following link http://localhost:8000/

Thank you for reading this blog.
