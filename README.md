# PHP World Locations

This library aims to define interfaces for standardization of query models for geographic locations.

## Installation

Add package to your composer.json by running:

```bash
composer require nekoos/world-locations
```

## Usage

Before using a location manager, it must be created:

```php
use NekoOs\World\Locations\Location;
use NekoOs\World\Locations\Manager;

/** @var Manager $manager */
Location::setManager($manager);            /* You can create your own [CustomManager] interface derived from 
                                            * [NekoOs\World\Locations\Manager] or implement the proposals 
                                            * [NekoOs\World\Locations\Proposals\EloquentManager]  
                                            * [NekoOs\World\Locations\Proposals\ApiManager] also customizable
                                            */ 

// create the location "manager" class
$country  = Location::getCountry();        /* Country Object */ 
$division = Location::getAdminDivision();  /* AdminDivision Object, admin division */
$district = Location::getDistrict();       /* District Object, minor subdivision */
```

Then it can be used the query method for one or more locations as needed:


### Usage as Eloquent

```php
$country->find('CO');                      /* Get Colombia object representation */
$country->find('CO')->getAdminDivisions(); /* All admin divisions from Colombia */
$country->find('CO')->getDistricts();      /* All districts from Colombia */

$division->find('CO-ATL');                 /* Get Atlántico-Colombia object representation */
$division->find('CO-ATL')->getDistrict();  /* All districts from Atlántico-Colombia */

$district->findAll();                      /* All districts from world */
```

### Usage as API

The api query interface tries to optimize requests in a single URI when possible, like this:

| Statement                                   | URI                                  | Description                            |
|---------------------------------------------|--------------------------------------|----------------------------------------|
| `$country->find('CO')->getDistricts()     ` | `/countries/CO/districts           ` | All districts from Colombia            |
| `$division->find('CO-ATL')->getDistricts()` | `/admin-divisions/CO-ATL/districts ` | All districts from Atlántico-Colombia  |
| `$district->findAll()                     ` | `/districts                        ` | All districts from world               |

### Usage with aliases

Some countries are formed by administrative divisions, such as states or departments, which in turn are formed by a 
grouping of districts, such as municipalities or provinces.

```php
$state   = Location::getState();           /* State Object, admin division aliases */
$city    = Location::getCity();            /* City Object, district aliases  */
```

| Statement                                   | URI                                  | Description                            |
|---------------------------------------------|--------------------------------------|----------------------------------------|
| `$country->find('CO')->getCities()        ` | `/countries/CO/cities              ` | All cities from Colombia               |
| `$state->find('CO-ATL')->getCities()      ` | `/states/CO-ATL/cities             ` | All cities from Atlántico-Colombia     |
| `$city->findAll()                         ` | `/cities                           ` | All cities from world                  |