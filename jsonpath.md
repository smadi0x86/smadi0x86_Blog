# JSONPATH

## JSON PATH in Kubectl

```bash
kubectl get pods -o json
```

#### To get the image in JSON path query:

```bash
kubectl get pods -o jsonpath=.items[0].spec.containers[0].image
```

## Custom Columns

```bash
kubectl get nodes -o=custom-columns=<COLUMN_NAME>:<JSON PATH>
```

```bash
kubectl get nodes -o=custom-columns=NODE:.metadata.name,CPU:status.capacity.cpu
```

## Sort By

#### To sort based on name:

```bash
kubectl get nodes --sort-by=.metadata.name
```
