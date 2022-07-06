# Release

* Update Makefile variable `VERSION` to the appropiate release version:
  * alpha: `VERSION ?= v1.15.5-alpha.3`, for operator development before doing a final release
  * stable: `VERSION ?= v1.15.5`, once alpha releases have been tested successfully

**IMPORTANT**: `VERSION` semver, **must coincide with the original helm chart release**,
because it's used to download the [original helm chart](https://artifacthub.io/packages/helm/banzaicloud-stable/vault-operator)
by the `download-helm-chart` makefile target.

For **alpha** releases, the only the semver prefix is used to fetch the upstream chart release.

## Alpha

**alpha** releases are used to test new versions of **the helm operator**,
when a new stable relase of the helm chart is published.

* If it is an **alpha** release, execute the following target to create appropiate `alpha` bundle files:

```bash
make prepare-alpha-release
```

* Then you can manually execute opetator, bundle and catalog build/push:

```bash
make container-build
make container-push
make bundle-publish
```

A real example of an alpha release can be found at [release: 1.15.6-alpha.1](https://github.com/3scale-ops/bank-vaults-helm-operator/pull/1/commits/9656d7034f5db124ab20afb4df12b85e9ee45bca) commit.

## Stable

* Execute the following target to create appropiate `alpha` and `stable` bundle files:

```bash
make prepare-stable-release
```
A real example of a stable release can be found at [release: 1.15.5](https://github.com/3scale-ops/bank-vaults-helm-operator/pull/1/commits/4411f1ad6b6b00dc2ac5fe9dc249a3a03ff1ef8e) commit.

* Then open a [Pull Request](https://github.com/3scale-ops/bank-vaults-helm-operator/pulls), and a GitHub Action will automatically detect if it is new release or not, in order to create it by building/pushing new operator, bundle and catalog images, as well as creating a GitHub release draft.