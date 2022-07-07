# Install

## Manual deploy

To manually install the operator (on all its dependant resources) on default
namespace `bank-vaults-helm-operator-system` without using OLM, you can use
the following make target:

```bash
make deploy
```

* Then create any [OperatorConfig resource type](../config/samples/operator_v1alpha1_operatorconfig.yaml).

```yaml
apiVersion: operator.vault.banzaicloud.com/v1alpha1
kind: OperatorConfig
metadata:
  name: example
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

* Once tested, delete created operator resources using the following make target:

```bash
make undeploy
```

## OLM manual deploy

If you want to install a specific version of the operator **manually** via OLM

* Deploy with `operator-sdk` using the following command:

```bash
operator-sdk run bundle quay.io/3scale/bank-vaults-helm-operator:v1.15.6
```

* Then create any [OperatorConfig resource type](../config/samples/operator_v1alpha1_operatorconfig.yaml):

```yaml
apiVersion: operator.vault.banzaicloud.com/v1alpha1
kind: OperatorConfig
metadata:
  name: example
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

* If you want to test an operator upgrade of a newer version, execute for example:

```bash
operator-sdk run bundle-upgrade quay.io/3scale/bank-vaults-helm-operator:v1.15.6
```

## OLM automatic deploy

If you want to install the operator via OLM on an **automatic** way subscribing
to a catalog, you can need to follow the following steps.

* First you need to deploy an specific `CatalogSource` in which operator releases will be published:

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
  name: bank-vaults-helm-operator-catalog
  namespace: vault
spec:
  sourceType: grpc
  image: quay.io/3scale/bank-vaults-helm-operator-catalog:latest
  displayName: Bank Vaults Helm Operator
  updateStrategy:
    registryPoll:
      interval: 30m
```

* Then you need to create an `OperatorGroup`, so you set the target namespaces in which the bank-vaults-helm-operator will watch for `OperatorConfig` custom resources (so it will be set operator ENVVAR `WATCH_NAMESPACE`):

```yaml
apiVersion: operators.coreos.com/v1alpha2
kind: OperatorGroup
metadata:
  name: vault
  namespace: vault
spec:
  targetNamespaces:
    - vault
```

* Finally create an operator `Subscription` on a given channel (`alpha`/`stable`) with `Automatic`/`Manual` installation (with `Manual` it will ask
you confirmation to install an operator upgrade):

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: bank-vaults-helm-operator
  namespace: vault
spec:
  channel: stable
  installPlanApproval: Manual
  name: bank-vaults-helm-operator
  source: bank-vaults-helm-operator-catalog
  sourceNamespace: vault
```

* Then create any [OperatorConfig resource type](../config/samples/operator_v1alpha1_operatorconfig.yaml):

```yaml
apiVersion: operator.vault.banzaicloud.com/v1alpha1
kind: OperatorConfig
metadata:
  name: vault
  namespace: vault
spec:
  resources:
    requests:
      cpu: 100m
      memory: 200Mi
    limits:
      cpu: 100m
      memory: 400Mi
```