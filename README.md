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
helm repo add yobasystems https://yobasystems.github.io/helm-charts
helm repo update
```

Install a chart
```bash
helm install my-database yobasystems/mariadb
```

