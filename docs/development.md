# Development

## Initial bootstrap

All operator files were initally bootstraped using `operator-sdk:v1.20.0` using:

```bash
$ operator-sdk init \
  --plugins helm \
  --group operator \
  --domain vault.banzaicloud.com \
  --kind OperatorConfig \
  --version v1alpha1 \
  --helm-chart=vault-operator \
  --helm-chart-repo="https://kubernetes-charts.banzaicloud.com" \
  --helm-chart-version=1.15.5
```

## Run it

To run the operator locally without creating any new image:

* You can run the operator locally watching all namespaces (default behaviour):

```bash
make run
```

* Or watching a specific namespace using envvar `WATCH_NAMESPACE`:

```bash
make run WATCH_NAMESPACE=example
```
