pimple-env-provider
===========
[![Build Status](https://travis-ci.org/e-artspace/pimple-env-provider.svg?branch=master)](https://travis-ci.org/e-artspace/resourceful)
[![Code Coverage](https://scrutinizer-ci.com/g/e-artspace/pimple-env-provider/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/e-artspace/pimple-env-provider/?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/e-artspace/pimple-env-provider/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/e-artspace/pimple-env-provider/?branch=master)

pimple-env-provider is a super simple, Silex compatible, lightweight service provider to allow overriding of Pimple container parameters with the value of environment variables.

# Install

```
> composer require e-artspace/pimple-env-provider
```

#Usage

The container's parameters allowed to be overridden must be declared in env.prefix array. Foreach parameter a cast service name must be supplied (custom or predefined).

Call 'env.overload' before using the container.

```php
putenv('DEBUG_APP=false');

$container = new \Pimple\Container([
	'debug.app'=> true
]);

$container->register(new \EnvProvider\EnvProvider([
	'env.vars'=>[
		'debug.app'=>'env.cast.boolean'
	]
]);

assert($container['debug.app'] === true);

$container['env.overload']; // this do the magic

assert($container['debug.app'] === false);
```

## Predefined services:

 - *env.cast.strval*: cast to a string
 - *env.cast.intval*: cast to a integer
 - *env.cast.json_decode*: to an array from a json string
 - *env.cast.boolean*: cast to boolean 'true','TRUE' oe '1' => true otherwhise  false
 - *env.name.builder*: function to generate environment variable name, by default: *environment var name* =  strtoupper((str_replace('.', '_', *container var name*)));
 - *env.prefix*: a prefix for generated environment variable name (default is empty) 

## Create a new cast service:

```php
$container['env.cast.onoff'] = $app->protect(function($str) {
	return (0===strcasecmp('on',$str));
};
```

## Customize *env.name.builder* service

```php
$container['env.name.builder'] = app->protect(function($name) {
	return strtolower((str_replace('.', '-', $name)));;
})
```


## Developing and Testing  with vagrant

A vagrant virtual appliance is available for developing and testing in a local workstation.

Local workstation requirements:

- install [GIT](http://git-scm.com/). Select “checkout as is , commit Unix-style line endings”.
- install [Vagrant](https://www.vagrantup.com/)
- install [Virtualbox](https://www.virtualbox.org/)

Open a bash shell and checkout pimple-env-provider project:

```shell
git clone https://github.com/e-artspace/pimple-env-provider.git
cd pimple-env-provider
```

The following commands can be used to start a virtual appliance, execute all tests and destroy virtual host:

```shell
vagrant up
vagrant ssh
cd /vagrant
composer install
vendor/bin/phpunit
exit
vagrant destroy
```

**Note that your git repo directory is mounted in /vagrant dir on the virtual host.**

