# pipe-dream/docs

## Platform overview
The bulk of Pipe Dream lives in pipe-dream/core. In addition, each language implementation has two repositories - one for logic, and one for package distribution

| repo  | summary |
| ------------- | ------------- |
| `pipe-dream/core`  | Interfaces, base classes, vue components and more  |
| `pipe-dream/laravel-file-factory`  | Logic specific to Laravel  |
| `pipe-dream/laravel-create` | A Laravel Package distributed with composer/packagist   |
| `...` | `...`   |
| `pipe-dream/<language>-file-factory` | Logic specific to `<language>`   |
| `pipe-dream/<language>-create` | Package (npm/gem/pip ...)   |



## Pipe Dream development guide
Using Laravel as example.

### 1. Install the core
```
git clone git@github.com:pipe-dream/core.git
cd core
yarn
```
Still in the `core`folder, prepare a link (so the other repos can access your local version of `core`)
```
yarn link
```

### 2. Install laravel-file-factory
```
git clone git@github.com:pipe-dream/laravel-file-factory.git
cd laravel-file-factory
yarn link core
yarn
```
Create a link for this repo as well
```
yarn link
```

### 2. Setup a host app and the Laravel package:
Create a fresh laravel project to act as host app:
```
laravel new pd-host
cd pd-host
```
Add our Laravel package in a dedicated folder:
```
mkdir -p packages/PipeDream
cd packages/PipeDream
git clone git@github.com:pipe-dream/laravel-beta.git Laravel
cd Laravel
composer install
yarn link core
yarn link laravel-factory
yarn
```
Add namespace to `pd-host/composer.json`:
```json
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "PipeDream\\Laravel\\": "packages/PipeDream/Laravel/src"
    },
```
And in the providers array of `pd-host/config/app.php` add:
```php
/*
* Package Service Providers...
*/
PipeDream\Laravel\PipeDreamServiceProvider::class,
```

Finally, you probably want to open up the three repos in three separate editors as well as terminal tabs.
The `core` have no build process, but `laravel-file-factory` and `laravel-create` needs to have `yarn watch` running.

Changes to `core`, `laravel-file-factory` and to the Laravel package itself should now instantly reflect when visiting `pd-host.test/pipe-dream`

Happy coding!
