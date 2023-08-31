# Laravel deploy script

Laravel deploy script that works as follows:

* go to site directory
* enable ssh
* add github ssh key
* reset the repo in case someone altered it locally and might make conflicts
* pull changes
* update composer
* migrate with force to prevent dialogues
* npm install to update or install js libs
* npm run build to compile everything
* cache the view and route

```
#!/bin/bash

cd /home/nicolay/portfolioWebsite
eval $(ssh-agent)
ssh-add ~/.ssh/github
git reset --hard
git pull
composer update
php artisan migrate --force
npm install
npm run build
php artisan view:cache
php artisan route:cache
```