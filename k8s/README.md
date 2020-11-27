## How to use

Kubernetes deployment is split in two parts to isolate what is specific to NetApp Harvest.

You must already have NetApp Harvest on your local machine, as downloaded from NetApp web site, and the NetApp Manageability SDK must have been properly installed in the `lib/` directory. Please consult NetApp Harvest documentation for how to do this.

### Graphite / Grafana Deployment

Simply apply the `graphite-grafana.yml` to your environemnt

```bash
# Create namespace if necessary
kubectl create ns netapp-harvest

# Deploy Graphite and Grafana
kubectl apply -n netapp-harvest -f graphite-grafana.yml
```

### Grafana Configuration

When the environment is ready, you still have to create an API key for NetApp Harvest and create a default data source.

You can use kubectl port-redirect to access Grafana

```sh
kubectl -n netapp-harvest port-forward grafana-0 3000
```

#### API key

You can access grafana on port 3000 of the pod, and generate an **Editor** API key, then set this key in `harvest.yml` in the NetApp Harvest container environment (don't forget to un-comment the lines)

#### Data source

Then go in Grafana data sources, and create a new default **Graphite** data source pointing to `http://graphite/`

### Deploy NetApp Harvest

You can now start NetApp Harvest

```bash
# Create namespace if necessary
kubectl -n netapp-harvest apply harvest.yml
```

#### Copy NetApp Harvest

As said in introduction, the Netapp Harvest container does **NOT** come with NetApp Harvest itself. Once you have a properly prepared netapp-harvest directory (as downloaded from NetApp web site, and with `lib/` directory prepared with NetApp Manageability SDK (See Netapp Hravest Documentation), you can copy it to the pod.

```bash
# Get pods
$ kubectl -n netapp-harvest get pod
NAME                       READY   STATUS    RESTARTS   AGE
grafana-0                  1/1     Running   0          10m
graphite-0                 1/1     Running   0          10m
harvest-75557d4469-t62lv   1/1     Running   0          6m1s

# Copy Netapp Harvest into the pod
$ kubectl cp netapp-harvest/ netapp-harvest/harvest-75557d4469-t62lv:/

# Delete pod to restart it and trigger dashboard installation
$ kubectl -n netapp-harvest delete pod harvest-75557d4469-t62lv

# Setup port redirect
$ kubectl -n netapp-harvest port-forward harvest-75557d4469-t62lv 5000
```

You can now access Swagger API on http://localhost:5000/ui/