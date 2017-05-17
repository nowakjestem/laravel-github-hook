Laravel GitHub Hook
===================

Easy continuous integration based on GitHub.

![Laravel-GitHub-Hook by HighSolutions](https://raw.githubusercontent.com/highsolutions/laravel-github-hook/master/intro.jpg)

Installation
------------

Add the following line to the `require` section of your Laravel webapp's `composer.json` file:

```javascript
    "require": {
        "HighSolutions/github-hook": "*"
    }
```

Run `composer update` to install the package.

Then, update `config/app.php` by adding an entry for the service provider:

```php
'providers' => [
    // ...
    HighSolutions\GitHubHook\GitHubHookServiceProvider::class,
];
```

Configuration
-------------

Finally, from the command line again, publish the default configuration file:

```bash
php artisan vendor:publish --provider="HighSolutions\GitHubHook\GitHubHookServiceProvider"
```

and you can specify some essentials settings (in ENV and in config file called `github-hook.php`):

| Setting name      | Description                                                                               | ENV variable               | Default value                             |
|-------------------|-------------------------------------------------------------------------------------------|----------------------------|-------------------------------------------|
| url               | URL for webhook from GitHub to your                                                       | GITHUB_HOOK_URL            | /github/hook/                             |
| branch            | Pulled repository branch on server                                                        | GITHUB_HOOK_BRANCH         | master                                    |
| secret            | GitHub webook secret code                                                                 | GITHUB_HOOK_SECRET         | null                                      |
| hooks.migration   | Artisan command for database migration. Set false to deactivate.                          | GITHUB_HOOK_HOOK_MIGRATION | php artisan migrate --force               |
| hooks.seed        | Artisan command for database seeding. Set false to deactivate.                            | GITHUB_HOOK_HOOK_SEED      | php artisan db:seed --force               |
| hooks.refresh     | Artisan command for recreating database (migration and seeding). Set false to deactivate. | GITHUB_HOOK_HOOK_REFRESH   | phpartisan migrate:refresh --seed --force |
| hooks.composer    | Composer update command (and dump-autoload). Set false to deactivate.                     | GITHUB_HOOK_HOOK_COMPOSER  | composer update --no-scripts              |
| slack.sender      | Name of sender of Slack notification                                                      | GITHUB_HOOK_SLACK_SENDER   | GitHub Hook                               |
| slack.channel     | Channel where Slack notification will be posted                                           | GITHUB_HOOK_SLACK_CHANNEL  |                                           |
| slack.webhook_url | Slack webhook url. If empty, notification won't be send.                                  | GITHUB_HOOK_SLACK_URL      |                                           |

No Laravel version
------------------

This package is also configured to work not within Laravel. We recommend:
- Create `hook` folder 
- Copy whole package to this folder
- Create another file (e.g. `awesome_hook.php`) and paste in it:

```php
<?php

require_once 'github-hook/src/Services/GitHubHookService.php';

HighSolutions\GitHubHook\Services\GitHubHookService::simpleInit([
    'secret' => null, // secret of webhook
    'branch' => 'master', // branch name
    'path' => '/home/www/', // absolute path to the repo
], $_POST['payload'], $_SERVER['x-hub-signature'], [
    'composer' => 'composer update --no-scripts',
]);
```

Configure GitHub webhook to access this second file and voila!

You can also upload only service file `github-hook/src/Services/GitHubHookService.php` and then you need to change path to:

```php
<?php

require_once 'GitHubHookService.php';

...
```

Changelog
---------

0.2.0

- no Laravel example
- no edit flag in git push
- support for ping request
- composer's hook
- hooks declarations
- conditional hooks based on committed files

0.1.0

- created package
- events
- slack notification
- hash verification

Roadmap
-------

* Comments
* Unit tests!

Credits
-------

This package is developed by [HighSolutions](http://highsolutions.pl), software house from Poland in love in Laravel.