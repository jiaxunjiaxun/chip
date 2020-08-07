# Mediawiki

## Wiki migrate

1. Dump data from old wiki

``` shell
php maintenance/dumpBackup.php --full --uploads > wiki-backup.xml
tar -czf wiki-images.tgz images
```

2. Import data from xml (very slow)

``` shell
php maintenance/importDump.php wiki-backup.xml

mkdir temporary
cp wiki-images.tgz temporary
cd temporary/
tar -xzf wiki-images.tgz
cd ../
mkdir tempimages
cp temporary/images/*/*/* tempimages
php maintenance/importImages.php tempimages
```

3. Refresh data (some states are not correct)

``` shell
php maintenance/initStats.php
php maintenance/initEditCount.php
php maintenance/rebuildrecentchanges.php
```

4. Fix missing image (images need rebuild)

``` shell
php maintenance/rebuildImages.php

php maintenance/rebuildImages.php --missing
```

5. Rebuild

``` shell
php maintenance/rebuildall.php
```
