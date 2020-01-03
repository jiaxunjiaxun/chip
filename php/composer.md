# Composer

## Include other project

Consider to include other project with *repositories* with type *path*

[Ref](https://getcomposer.org/doc/05-repositories.md#private-packagist)


``` shell
# global set
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
# global remove
composer config -g --unset repos.packagist

# project set
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
# project remove
composer config --unset repos.packagist
```
