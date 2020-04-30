
# Subscription sample

As with channels, subscriptions (`subscription.apps.open-cluster-management.io`) provide you with improved continuous integration and continuous delivery capabilities for application management.

Subscriptions are sets of definitions that identify Helm charts, deployables, and other Kubernetes resources within channels by using annotations, labels, and versions. Subscriptions can point to a channel or storage location for identifying new or updated deployables. The subscription operator can then download the subscribed Helm chart, deployable, or secret directly from the storage location to target managed clusters without checking the Hub cluster first. With a subscription, the subscription operator can monitor the channel for new or updated resources instead of the Hub cluster.

The definition structure for a subscription can resemble the following YAML content:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: nginx
  namespace: ns-sub-1
  labels:
    app: nginx-app-details
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
  packageFilter:
    version: "1.36.x"
  placement: # See the following section for information
    placementRef:
      kind: PlacementRule
      name: towhichcluster
  overrides: # See Deployable documentation
  - clusterName: "/"
    clusterOverrides:
    - path: "metadata.namespace"
      value: default
```

For more information about creating and managing subscriptions, see [Managing subscriptions](managing_subscriptions.md).

## Placement rules

Placement rules (`placementrule.apps.open-cluster-management.io`) define the target clusters where deployables can be deployed. Use placement rules to help you facilitate the multi-cluster deployment of your deployables.

The custom resource definition (CRD) and controller for placement rules replaces the placement policies that were used for applications in previous versions of Red Hat Advanced Cluster Management for Kubernetes. Placement policies are still used for governance and risk policies.

Placement rules can be defined for subscriptions and for deployables. Define the placement rule at the subscription level for multi-cluster deployments. Define the placement rule for a specific deployable for single-cluster deployments or to override placement settings.  

The definition structure for a placement rule can resemble the following YAML content:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: PlacementRule
metadata:
  name: towhichcluster
  namespace: ns-sub-1
spec:
  clusterSelector: {}
```

For more information about creating and managing placement rules, see [Managing placement rules](managing_placement_rules.md).