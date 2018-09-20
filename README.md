# cma-proxy-chart
cluster to cluster kubernetes api proxying 3 different ways

## Summary

Sets up a proxy to remote kubernetes api endpoints three different ways:

1) Using Nginx, `kubectl proxy` sidecars and secrets containing kubeconfigs
2) Using Nginx, bearer tokens and known K8s API endpoint array
3) Using Ngninx, bearer tokens and `X-Target-Cluster` custom header

## With kubectl

* create secrets with your remote cluster kubeconfig's
* setup the `clusters:` value:

```
clusters:
  - id: cluster-id 
    secret: secret-name 
    secretkey: key-of-kubeconfig-data
  ...
```
* setup `basicAuth` key
* install chart
* query remote cluster like so: 'curl http://<external service ip>/cluster-id/api/v1/pods -u user:pass'

## With Nginx, bearer tokens and known K8s API endpoint array

* setup the `clusters:` value:

```
clusters:
  - id: cluster-id 
    endpoint: https://1.2.3.4
  ...
```

* install chart
* query remote cluster like so: 'curl http://<external service ip>/cluster-id/api/v1/pods --header "Authorization: Bearer <YOUR BEARER TOKEN>"'

## With Ngninx, bearer tokens and `X-Target-Cluster` custom header

* install chart
* query remote cluster like so: 'curl http://<external service ip>/cluster-id/api/v1/pods --header "Authorization: Bearer <YOUR BEARER TOKEN>" --header "X-Target-Cluster: <YOUR API ENDPOINT>"'