# zubzet/dockerfiles

Opinionated PHP+Apache Docker base images for the [ZubZet framework](https://github.com/zubzet/framework) and downstream projects.

Three variants per PHP version (8.0–8.5):

| Tag | Base | Use case |
| --- | ---- | -------- |
| `8.X` | `php:8.X-cli` + extensions + Composer | CLI workers, cron containers |
| `8.X-apache` | `php:8.X-apache` + extensions + Composer + opinionated vhost | Web app containers (mod_php) |
| `8.X-xdebug` | `8.X-apache` + Xdebug 3 | Local dev / debugging |

All images ship with the same set of PHP extensions:

```
bcmath bz2 calendar curl exif gd intl mbstring
mysqli pdo_mysql sockets iconv session zip
xmlreader xmlwriter simplexml
```

The Apache variant bakes in a vhost serving from `/var/www/html/webroot` with `AllowOverride All` and `mod_rewrite` enabled.

## Registries

Published to both:

- **GitHub Container Registry**: `ghcr.io/zubzet/php:8.X[-apache|-xdebug]`
- **Docker Hub**: `zubzet/php:8.X[-apache|-xdebug]`

Multi-arch: `linux/amd64` + `linux/arm64`.

> Note: Docker Hub anonymous pulls are rate-limited (100 / 6h / IP). Authenticated CI runs are unaffected, but high-volume anonymous consumers should prefer GHCR or `docker login`.

## Use it

```dockerfile
FROM ghcr.io/zubzet/php:8.4-apache

# Adjust UID for host volume mounts (optional)
ARG USER_UID=1000
USER root
RUN usermod -u "$USER_UID" www-data \
 && groupmod -g "$USER_UID" www-data \
 && chown -R www-data:www-data /var/www /var/log/apache2 /run/apache2
USER www-data

WORKDIR /var/www/html
```

## Update cadence

Images are rebuilt:

- on every push to `main` that touches `php/**`
- on `workflow_dispatch`
- monthly via cron (1st of month, 03:00 UTC) to pick up upstream `php:8.X-apache` point releases

## Layout

Mirrors the `apko/melange` structure of the company-internal source repo, so swapping to that source later is mechanical:

```
php/
  v8.0/{php,apache,xdebug}/Dockerfile
  v8.1/...
  v8.5/...
  config-apache/site-000-default.conf
  config-xdebug/xdebug.ini
```

## License

Apache 2.0 — see [LICENSE](./LICENSE).
