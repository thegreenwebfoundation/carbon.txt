## Implementing a `CarbonTxt-Location` HTTP header with Apache HTTPD for carbon.txt

Consider the scenario where you are operate a managed WordPress service with a website at managed-service.com, but you serve your customer's websites at their own domain, like https://downstream-customer.com.



You use [Apache](https://httpd.apache.org/) as a web server, which listens for inbound requests to https://downstream-customer.com, and serves both static files, and php files, via Fast-CGI.


### A very simplified diagram of this set up

```mermaid
flowchart LR

    browser
    browser-->apache

```

Let's assume you have set up apache according to the [Wordpress documentation](https://developer.wordpress.org/advanced-administration/server/web-server/httpd/), and have the following configuration in an .htaccess or configuration file:


```apacheconf

# BEGIN WordPress

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]

# END WordPress

```

To add HTTP `CarbonTxt-Location` header, where you can point to a specific location for a carbon.txt file, you would add the following lines, using the [Header](https://httpd.apache.org/docs/current/mod/mod_headers.html) directive.


```apacheconf
<IfModule mod_headers.c>
    Header set CarbonTxt-Location https://managed-service.com/carbon.txt
</IfModule>
```


```apacheconf

# BEGIN WordPress

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]

# END WordPress

# BEGIN carbon.txt delegation

<IfModule mod_headers.c>
    Header set CarbonTxt-Location https://managed-service.com/carbon.txt
</IfModule>

# END carbon.txt delegation
```

This also requires the [`mod_headers`](https://httpd.apache.org/docs/current/mod/mod_headers.html) apache module to be enabled, which can be done by running the following command from the shell as a user with superuser privileges:

```
sudo a2enmod headers
```



## Further examples

This is an open source repository - if you're looking for specific example, or would like to contribute one, [please open an issue](https://github.com/thegreenwebfoundation/carbon.txt/issues).
