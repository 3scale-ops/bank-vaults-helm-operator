# Bank-Vaults Vault helm operator

[![test](https://github.com/3scale-ops/bank-vaults-helm-operator/actions/workflows/test.yaml/badge.svg)](https://github.com/3scale-ops/bank-vaults-helm-operator/actions/workflows/test.yaml)
[![build](https://github.com/3scale-ops/bank-vaults-helm-operator/actions/workflows/release.yaml/badge.svg)](https://github.com/3scale-ops/bank-vaults-helm-operator/actions/workflows/release.yaml)
[![release](https://badgen.net/github/release/3scale-ops/bank-vaults-helm-operator)](https://github.com/3scale-ops/bank-vaults-helm-operator/releases)
[![license](https://badgen.net/github/license/3scale-ops/bank-vaults-helm-operator)](https://github.com/3scale-ops/bank-vaults-helm-operator/blob/main/LICENSE)

A Kubernetes Operator based on the Operator SDK (Helm version) to configure **[official banzaicloud/bank-vaults Vault operator helm chart](https://artifacthub.io/packages/helm/banzaicloud-stable/vault-operator)**, so it can be installed via OLM without having to do any change on current Helm Charts.

The usual Helm Chart file `values.yaml`, like:

```yaml
image:
  bankVaultsRepository: ghcr.io/banzaicloud/bank-vaults
  repository: ghcr.io/banzaicloud/vault-operator
resources:
  requests:
    cpu: 100m
    memory: 200Mi
  limits:
    cpu: 100m
    memory: 400Mi
```

Need to be encapsulated into a new custom resource called `OperatorConfig`:

```yaml
apiVersion: operator.vault.banzaicloud.com/v1alpha1
kind: OperatorConfig
metadata:
  name: cluster
spec:
  image:
    bankVaultsRepository: ghcr.io/banzaicloud/bank-vaults
    repository: ghcr.io/banzaicloud/vault-operator
  resources:
    requests:
      cpu: 100m
      memory: 200Mi
    limits:
      cpu: 100m
      memory: 400Mi
```

So the operator will create all helm chart resources, using the custom resource name as a prefix for all resources names.

## Quick release

You can easily generate the operator for any particular version of the Cert
Manager Helm Chart using the `container-build` target with a specific `VERSION`.

- Command

```bash
VERSION=v1.15.6 make container-build
```
```bash
banzaicloud-stable/vault-operator@v1.15.6 downloaded to /home/rael/gh/3scale-ops/bank-vaults-helm-operator/helm-charts
2022/07/06 13:20:32 resource bases/vault.banzaicloud.com_crds.yaml already in kustomization file
docker build -t quay.io/3scale/bank-vaults-helm-operator:vv1.15.6 .
Sending build context to Docker daemon  253.4MB
Step 1/5 : FROM quay.io/operator-framework/helm-operator:v1.20.0
 ---> 0612bfda4e55
Step 2/5 : ENV HOME=/opt/helm
 ---> Using cache
 ---> 28509b255293
Step 3/5 : COPY watches.yaml ${HOME}/watches.yaml
 ---> Using cache
 ---> 57abef551e9f
Step 4/5 : COPY helm-charts  ${HOME}/helm-charts
 ---> Using cache
 ---> deebeb5ef7ad
Step 5/5 : WORKDIR ${HOME}
 ---> Running in aa0360f4d23e
Removing intermediate container aa0360f4d23e
 ---> d45c0dbfda24
Successfully built d45c0dbfda24
Successfully tagged quay.io/3scale/bank-vaults-helm-operator:vv1.15.6
```

- Files generated

```bash
$ grep version helm-charts/vault-operator/Chart.yaml
version: 1.15.6
$ tree helm-charts/
helm-charts/
├── README.md
└── vault-operator
    ├── Chart.yaml
    ├── README.md
    ├── templates
    │   ├── _helpers.tpl
    │   ├── deployment.yaml
    │   ├── psp.yaml
    │   ├── role.yaml
    │   ├── rolebinding.yaml
    │   ├── sa.yaml
    │   ├── service.yaml
    │   └── servicemonitor.yaml
    └── values.yaml

2 directories, 12 files
```

For more detailed information, check the [documentation](docs/) available.

## Documentation

* [Install](docs/install.md)
* [Development](docs/development.md)
* [Release](docs/release.md)

## Contributing

You can contribute by:

* Raising any issues you find using Bank-Vaults Vault helm operator
* Fixing issues by opening [Pull Requests](https://github.com/3scale-ops/bank-vaults-helm-operator/pulls)
* Submitting a patch or opening a PR
* Improving documentation
* Talking about Bank-Vaults Vault helm operator

All bugs, tasks or enhancements are tracked as [GitHub issues](https://github.com/3scale-ops/bank-vaults-helm-operator/issues).

## License

Bank-Vaults Vault helm operator is under Apache 2.0 license. See the [LICENSE](LICENSE) file for details.