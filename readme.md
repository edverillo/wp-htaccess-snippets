# Useful .htaccess Snippets for WordPress

This is a collection of useful snippets extracted from various sources. Contributions are welcome!

## Default

```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

## Deny access to all .htaccess files

```
# Denies access to all .htaccess files
<Files ~ "^.*\.([Hh][Tt][Aa])">
Order Allow,Deny
Deny from all
Satisfy all
</Files>
```

## Protect WP config

```
# Protects wp-config
<Files wp-config.php>
Order Allow,Deny
# Allow from xx.xx.xx.xxx
# Allow from yy.yy.yy.yyy
Deny from all
</Files>
```

## Prevent XML-RPC DDoS attack

```
# Protects XML-RPC, prevents DDoS attack
<FilesMatch "^(xmlrpc\.php)">
Order Deny,Allow
# Allow from xx.xx.xx.xxx
# Allow from yy.yy.yy.yyy
Deny from all
</FilesMatch>
```

## Protect admin area

```
# Protects admin area by IP
AuthUserFile /dev/null
AuthGroupFile /dev/null
AuthName "WordPress Admin Access Control"
AuthType Basic
<LIMIT GET>
Order Deny,Allow
Deny from all
Allow from xx.xx.xx.xxx
Allow from yy.yy.yy.yyy
</LIMIT>
```

## Prevent directory listing

```
# Prevents directory listing
Options -Indexes
```

## Prevent username enumeration

```
# Prevents username  enumeration
RewriteCond %{QUERY_STRING} author=d
RewriteRule ^ /? [L,R=301]
```

## Block spammers and bots

```
# Blocks spammers and bots
<Limit GET POST>
Order Allow,Deny
Deny from xx.xx.xx.xxx
Deny from yy.yy.yy.yyy
</Limit>
Allow from all
```

## Prevent image hotlinking

```
# Prevents image hotlinking
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?yourwebsite.com [NC]
RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?yourwebsite2.com [NC]
RewriteRule \.(jpe?g?|png|gif|ico|pdf|flv|swf|gz)$ - [NC,F,L]
```

## Restrict direct access to plugin & theme PHP files

```
# Restricts access to PHP files from plugin and theme directories
RewriteCond %{REQUEST_URI} !^/wp-content/plugins/file/to/exclude\.php
RewriteCond %{REQUEST_URI} !^/wp-content/plugins/directory/to/exclude/
RewriteRule wp-content/plugins/(.*\.php)$ - [R=404,L]
RewriteCond %{REQUEST_URI} !^/wp-content/themes/file/to/exclude\.php
RewriteCond %{REQUEST_URI} !^/wp-content/themes/directory/to/exclude/
RewriteRule wp-content/themes/(.*\.php)$ - [R=404,L]
```

## Permanent redirects 

```
# Permanent redirects
Redirect 301 /oldurl1/ http://yoursite.com/newurl1
Redirect 301 /oldurl2/ http://yoursite.com/newurl2
```

## Maintenance mode

```
# Redirects to maintenance page
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{REMOTE_ADDR} !^123\.456\.789\.000
RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif) [NC]
RewriteRule .* /maintenance.html [R=503,L]
</IfModule>
```

## Restrict all access to wp-includes

```
# Blocks all wp-includes folders and files
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>
```

## Block XSS

```
# Blocks some XSS attacks
<IfModule mod_rewrite.c>
RewriteCond %{QUERY_STRING} (\|%3E) [NC,OR]
RewriteCond %{QUERY_STRING} GLOBALS(=|\[|\%[0-9A-Z]{0,2}) [OR]
RewriteCond %{QUERY_STRING} _REQUEST(=|\[|\%[0-9A-Z]{0,2})
RewriteRule .* index.php [F,L]
</IfModule>
```

## Enable browser caching

```
# Enables browser caching
<IfModule mod_expires.c>
ExpiresActive On
ExpiresByType image/jpg "access 1 year"
ExpiresByType image/jpeg "access 1 year"
ExpiresByType image/gif "access 1 year"
ExpiresByType image/png "access 1 year"
ExpiresByType text/css "access 1 month"
ExpiresByType application/pdf "access 1 month"
ExpiresByType text/x-javascript "access 1 month"
ExpiresByType application/x-shockwave-flash "access 1 month"
ExpiresByType image/x-icon "access 1 year"
ExpiresDefault "access 2 days"
</IfModule>
```

## Custom error pages

```
# Sets up custom error pages
ErrorDocument 403 /custom-403.html
ErrorDocument 404 /custom-404.html
```