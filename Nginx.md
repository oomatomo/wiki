# Nginx

## Basic認証

```
location / {
  auth_basic    "Basic";
  auth_basic_user_file    "/var/www/htpasswd";
}
```
