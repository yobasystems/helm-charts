# Helm Charts for Yoba Systems

This repository contains Helm charts for Yoba Systems.

## Charts

- `yobasystems/mariadb`
- `yobasystems/nginx`
- `yobasystems/redis`
- `yobasystems/wordpress`
- `yobasystems/postgresql`
- `yobasystems/caddy`
- `yobasystems/prestashop`
- `yobasystems/ghost`

## Usage

Add the helm chart repo & update

```bash
helm repo add yobasystems https://helm-charts.yoba.systems
helm repo update
```

Install a chart

```bash
helm install my-mariadb yobasystems/mariadb
```

## Development

### Updating the repo package

```bash
helm package mariadb
helm package nginx
helm package postgresql
```

### Updating the repo index

```bash
helm repo index . --url https://helm-charts.yoba.systems
```

## More info

- [mariadb](mariadb/README.md)
- [nginx](nginx/README.md)
- [redis](redis/README.md)
- [wordpress](wordpress/README.md)
- [postgresql](postgresql/README.md)
- [caddy](caddy/README.md)
- [prestashop](prestashop/README.md)
- [ghost](ghost/README.md)
