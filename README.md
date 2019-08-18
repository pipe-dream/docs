# pipe-dream/docs

## Platform overview
The bulk of Pipe Dream lives in pipe-dream/core. In addition, each language implementation has two repositories - one for logic, and one for package distribution

### Repositories 
| repo  | summary |
| ------------- | ------------- |
| `pipe-dream/core`  | Interfaces, base classes, vue components and more  |
| `pipe-dream/laravel-file-factory`  | Logic specific to Laravel  |
| `pipe-dream/laravel-create` | A Laravel Package distributed with composer/packagist   |
| `...` | `...`   |
| `pipe-dream/<language>-file-factory` | Logic specific to `<language>`   |
| `pipe-dream/<language>-create` | Package (npm/gem/pip ...)   |

### Overview
<a href="https://www.lucidchart.com/documents/edit/e228254d-6c72-43ba-9bf2-0cd81be2cce6/0?beaconFlowId=CF13F2391D43F2E2" target="_blank">
    <img src="https://user-images.githubusercontent.com/3457668/63221673-b52a0680-c19c-11e9-946e-d0fe8cd85e10.png">
</a>

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

### 3. Setup a host app and the Laravel package:
Create a fresh laravel project to act as host app:
```
laravel new pd-host
cd pd-host
```
Add our Laravel package in a dedicated folder:
```
mkdir -p packages/PipeDream
cd packages/PipeDream
git clone git@github.com:pipe-dream/laravel-create.git LaravelCreate
cd Laravel
composer install
yarn link core
yarn link laravel-file-factory
yarn
```
Add namespace to `pd-host/composer.json`:
```json
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "PipeDream\\LaravelCreate\\": "packages/PipeDream/LaravelCreate/src"
    },
```
And in the providers array of `pd-host/config/app.php` add:
```php
/*
* Package Service Providers...
*/
PipeDream\LaravelCreate\LaravelCreateServiceProvider::class,
```

Finally, you probably want to open up the three repos in three separate editors as well as terminal tabs.
The `core` have no build process, but `laravel-file-factory` and `laravel-create` needs to have `yarn watch` running.

Changes to `core`, `laravel-file-factory` and to the Laravel package itself should now instantly reflect when visiting `pd-host.test/pipe-dream`

Happy coding!
