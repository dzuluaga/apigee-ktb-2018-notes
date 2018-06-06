#### Create cluster

Use Google Deployment to deploy Istio.

#### Step 1: Get services

```bash
$ kubectl get deployments,ing -n istio-system

Response:

NAME                            DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/grafana                  1         1         1            1           5m
deploy/istio-ca                 1         1         1            1           5m
deploy/istio-ingress            1         1         1            1           5m
deploy/istio-mixer              1         1         1            1           5m
deploy/istio-pilot              1         1         1            1           5m
deploy/istio-sidecar-injector   1         1         1            1           5m
deploy/prometheus               1         1         1            1           5m
deploy/servicegraph             1         1         1            1           5m
deploy/zipkin                   1         1         1            1           5m
```

#### Step 2: Confirm that application is deployed

Check number of pods per service. Replicas.
```bash
$ kubectl get deployments,ing

Response:

NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/details-v1       1         1         1            1           6m
deploy/productpage-v1   1         1         1            1           6m
deploy/ratings-v1       1         1         1            1           6m
deploy/reviews-v1       1         1         1            1           6m
deploy/reviews-v2       1         1         1            1           6m
deploy/reviews-v3       1         1         1            1           6m
NAME          HOSTS     ADDRESS           PORTS     AGE
ing/gateway   *         130.211.112.212   80        6m
```

#### Step 4: Show K8s Cluster through UI

#### Step 5: Show Services through UI

#### Step 6: For each service show number of pods

#### Step 7: Show Istio Ingress and resources mapped to it

#### Step 8; Show actual GCP Load Balance

  #### Step 9: Show port 80 and 443 in load balancer

#### Step 10: Show productsdetail workload and show node, then show GCE VM

#### Step 11: Access Bookinfo IP Address

```bash
$ kubectl get ingress -o wide

Response:
NAME      HOSTS     ADDRESS           PORTS     AGE
gateway   *         130.211.112.212   80        26m
```

#### Step 12: Visit Product Page

```
http://${GATEWAY_URL}/productpage
```

#### Step 13: Send some traffic

for i in {1..100}; do curl -o /dev/null -s -w "%{http_code}\n" http://130.211.112.212/productpage; done


#### Step 14: Verify Istio Grafana plugin

```bash
$ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
```
#### Step 15: Istio Dashboard
```
http://localhost:3000/dashboard/db/istio-dashboard
```

#### Step 16: Expose and view Prometheus

```
$ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &

http://localhost:9090/graph
```

#### Step 17: Show Service Graph

```
$ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088 &

http://localhost:8088/dotviz
https://8088-dot-2982202-dot-devshell.appspot.com/dotviz
```

#### Step 18: Tracing with Zipkin show off trace for a page up to the request detail

```
$ kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=zipkin -o jsonpath='{.items[0].metadata.name}') 8080:9411 &

http://localhost:8080
```

#### Step 19: Create a route rule:


Get rules:
```
$ istioctl get routerules -o yaml
```

```
$ istioctl create -f samples/bookinfo/kube/route-rule-all-v1.yaml -n default
```

#### Step 20: Delete route rules
istioctl delete -f samples/bookinfo/kube/route-rule-all-v1.yaml -n default

#### Step 21: To test reviews:v2, but only for a certain user (jason)

Notice star reviews for jason are always black star, while for everything else is round roubin.
```
istioctl create -f samples/bookinfo/kube/route-rule-reviews-test-v2.yaml -n default
```