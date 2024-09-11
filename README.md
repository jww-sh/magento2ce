# Magento 2 Community Edition for Platform.sh/Upsun

<p align="center">
<a href="https://console.platform.sh/projects/create-project?template=https://raw.githubusercontent.com/platformsh/template-builder/master/templates/magento2ce/.platform.template.yaml&utm_content=magento2ce&utm_source=github&utm_medium=button&utm_campaign=deploy_on_platform">
    <img src="https://platform.sh/images/deploy/lg-blue.svg" alt="Deploy on Platform.sh" width="180px" />
</a>
</p>

This template builds Magento 2 CE on Platform.sh and Upsun.  It includes the Magento ECE-Tools to run effectively in a build-and-deploy environment.  A MariaDB Database, Elasticsearch Indexer, RabbitMQ Message Queue and Redis Cache server come pre-configured and work out of the box. 

Magento is a fully integrated ecommerce system and web store written in PHP.  This is the Open Source version of Magento.

## Features

* PHP 8.3
* MariaDB 10.6
* Redis 7.2
* Opensearch
* RabbitMQ 3.13
* Automatic TLS certificates
* Composer-based build

## Warning

This template will fail when first deployed with the following error

```
Please configure your COMPOSER_AUTH enviromental variable as per the README file
```
Please follow the post install instructions and add the needed authentication for composer.

## Composer Authentication and Post Installation Setup

1. Get your Magento Repository authentication keys https://devdocs.magento.com/guides/v2.4/install-gde/prereq/connect-auth.html
2. Add your keys as a project level variable `platform variable:create -p <your Platform.sh projectID> --level project --name env:COMPOSER_AUTH --json true --visible-runtime false --sensitive true --visible-build true  --value '{"http-basic":{"repo.magento.com":{"username":"<your public key>","password":"<your private key>"}}}'`    
3. Please add an admin user using `php bin/magento admin:user:create`.  Login at `/admin` in your browser. 
4. If you need to disable Magento two factor auth for admin logins on development enviroments with mail disabled, please SSH into your application and run `bin/magento config:set twofactorauth/general/enable 0` 

## Customizations

The following changes have been made relative to Magento 2 as it is downloaded from Magento.com.  If using this project as a reference for your own existing project, replicate the changes below to your project.

* The `.platform.app.yaml`, `.platform/services.yaml`, and `.platform/routes.yaml` files have been added.  These provide Platform.sh-specific configuration and are present in all projects on Platform.sh.  You may customize them as you see fit.
* The `.upsun/config.yaml` files has been added. This provides Upsun-specific configuration which can also be reviewed.
* The `composer.json` file has had the ECE-Tools package and its dependencies added.
* Magento crons have been setup to ensure they are run sequentially to ensure there is availible memory
* A logrotate and report housekeeping cron have been added.
* A module which allows two factor authentication to be disabled has been added to `composer.json`.

## References

* [Magento](https://magento.com/)
* [PHP on Platform.sh](https://docs.platform.sh/languages/php.html)
