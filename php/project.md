# Project

## Create project

```shell
composer init
# Package name - <vendor>/<name>
# Description
# Author - name <email>
# License - MIT
# Minimum Stability - dev
# Package Type - project
# Requires & Development Requires
# Namespace <Vendor>/<Package>

composer install
composer update
```

## Development

```
public
    └─ index.php

src
    ├─ BootstrapClass.php [namespace Vendor\Project]
    └─ package
        └─ ActionClass.php [namespace Vendor\Project\Package]
```
