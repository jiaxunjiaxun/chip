# Wordpress

## Theme

[Ref - 1](http://www.ludou.org/create-wordpress-themes-prepare.html)

[Ref - 2](https://developer.wordpress.org/themes/basics/template-hierarchy/)

[Ref - 3](https://themeshaper.com/2012/10/22/the-themeshaper-wordpress-theme-tutorial-2nd-edition/)
![Wordpress Page](./res/wrodpress-page.png)

### Home (Required)
home.php -> index.php

### Article
single-{post_type}.php (v3.0+). single-videos.php for Video type
single.php -> index.php

### Custom Template
page-{slug}.php (v2.9+)
page-{id}.php
page.php -> index.php

### Category
category-{slug}.php (v2.9+)
category-{id}.php
category.php -> archive.php -> index.php

### Tag
tag-{slug}.php (v2.9+)
tag-{id}.php
tag.php -> archive.php -> index.php

### Author
author-{nicename}.php (v3.0)
author-{id}.php
author.php -> archive.php -> index.php

### Date
date.php -> archive.php -> index.php

### Search
search.php -> index.php

### 404
404.php -> index.php

### Attachment
{MIME_type}.php -> attachment.php -> single.php -> index.php