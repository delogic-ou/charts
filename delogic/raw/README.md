# delogic/raw

The [delogic/raw](https://github.com/delogic-io/charts/tree/main/delogic/raw) chart takes a list of Kubernetes resources and merges each resource with a default `metadata.labels` map and installs the result. Use this chart to generate arbitrary Kubernetes manifests instead of kubectl and scripts.

The Kubernetes resources can be "raw" ones defined under the `resources` key, or "templated" ones defined under the `templates` key.

## Usage

### Raw resources

#### Step 1: Create a yaml file containing your raw resources.

Resources list can contain mixed types, i.e. include strings and maps simultaneously.

```
# raw-priority-classes.yaml
resources:
  - apiVersion: scheduling.k8s.io/v1beta1
    kind: PriorityClass
    metadata:
      name: common-critical
    value: 100000000
    globalDefault: false
    description: "This priority class should only be used for critical priority common pods."

  - apiVersion: scheduling.k8s.io/v1beta1
    kind: PriorityClass
    metadata:
      name: common-critical-from-string
    value: 100000000
    globalDefault: false
    description: "This priority class should only be used for critical priority common pods."
```

#### Step 2: Install your raw resources.

```
helm install raw-priority-classes delogic/raw -f raw-priority-classes.yaml
```

### Templated resources

#### Step 1: Create a yaml file containing your templated resources.

```
# values.yaml

templates:
- |
  apiVersion: v1
  kind: Secret
  metadata:
    name: common-secret
  stringData:
    mykey: {{ .Values.mysecret }}
```

#### Step 2: Install your templated resources.

```
helm install mysecret delogic/raw -f values.yaml
```
