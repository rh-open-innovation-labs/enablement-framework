# DO500 Cluster Tooling

This directory contains the necessary charts used in order to deploy a DO500 Tech Stack against an OCP 4.X cluster. This assumes that the cluster has valid certificates.

This chart is capable of deploying the following:

- Gitlab (version X.Y.Z)
- CodeReady Workspaces (version X.Y.Z)
- Instructor Documentation
- SealedSecrets from Bitnami
- OpenShift Pipelines
- Advanced Cluster Security (StackRox)
- Cluster Logging (ELK)
- Certificate Utils

## Installation

```shell
cd ./tooling/charts/do500
helm dep up
helm upgrade --install do500 . --namespace do500 --create-namespace --timeout=15m
oc apply -f https://raw.githubusercontent.com/rht-labs/enablement-framework/main/tooling/charts/do500/templates/do500-rbac.yaml
```

Sample output commands:

```shell
❯ helm dep up
Getting updates for unmanaged Helm repositories...
...Successfully got an update from the "https://bitnami-labs.github.io/sealed-secrets" chart repository
...Successfully got an update from the "http://rht-labs.com/stackrox-chart" chart repository
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "prometheus-community" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
Saving 2 charts
Downloading sealed-secrets from repo https://bitnami-labs.github.io/sealed-secrets
Downloading stackrox-chart from repo http://rht-labs.com/stackrox-chart
Deleting outdated charts
❯ helm upgrade --install do500 . --namespace do500 --create-namespace --timeout=15m
Release "do500" does not exist. Installing it now.
NAME: do500
LAST DEPLOYED: Fri Nov 12 17:17:56 2021
NAMESPACE: do500
STATUS: deployed
REVISION: 1
TEST SUITE: None
❯ oc apply -f https://raw.githubusercontent.com/rht-labs/enablement-framework/main/tooling/charts/do500/templates/do500-rbac.yaml
clusterrole.rbac.authorization.k8s.io/do500-clusterrole created
clusterrolebinding.rbac.authorization.k8s.io/do500-clusterrolebinding created
rolebinding.rbac.authorization.k8s.io/do500-users-edit created
rolebinding.rbac.authorization.k8s.io/do500-workspaces-view created
rolebinding.rbac.authorization.k8s.io/do500-gitlab-view created
rolebinding.rbac.authorization.k8s.io/do500-shared-view created
Error from server (NotFound): error when creating "https://raw.githubusercontent.com/rht-labs/enablement-framework/main/tooling/charts/do500/templates/do500-rbac.yaml": namespaces "ipa" not found
```

## Deleting

To delete:
```bash
helm uninstall do500 --namespace do500
```

## Gitlab

With Gitlab, it expects to be able to run against a configured LDAP server. This can be achieved by either uncommenting and providing the appropriate values in your `values.yaml` or you can allow the helm chart to discover these values itself.

After this is deployed, you will have a functional gitlab server that can be used along with your LDAP identities.

## CodeReady Workspaces

With CRW, this uses the provided Operator to deploy a CRW instance. With the provided defaults, it restricts uses to two workspaces and allows for only a single `running` instance.
