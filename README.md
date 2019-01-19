# MySQL single master operator
a namespace scoped mysql operator

## Quick start

1. Prepare CRD
```
$ export NAMESPACE="mysql-test"
$ kubectl create namespace $NAMESPACE
$ kubectl -n $NAMESPACE create deploy/crds/lemonmysql_v1alpha1_mysql_crd.yaml
$ kubectl -n $NAMESPACE create deploy/service_account.yaml
$ kubectl -n $NAMESPACE create deploy/role.yaml
$ kubectl -n $NAMESPACE create deploy/role_binding.yaml
```
2. Deploy operator pod
```
$ kubectl -n $NAMESPACE create -f deploy/operator.yaml
$ kubectl -n $NAMESPACE get deployment
NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
mysql-operator     1         1         1            1           1m
```
3.Create a mysql CR
```
$ kubectl -n $NAMESPACE apply -f deploy/crds/lemonmysql_v1alpha1_mysql_cr.yaml
$ kubectl -n $NAMESPACE get mysql
NAME            AGE
example-mysql   20m
$ kubectl -n $NAMESPACE get mysql -o yaml
apiVersion: v1
items:
- apiVersion: lemonmysql.io/v1alpha1
  kind: Mysql
  metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"lemonmysql.io/v1alpha1","kind":"Mysql","metadata":{"annotations":{},"name":"example-mysql","namespace":"default"},"spec":{"rootpass":"hellopass"}}
    creationTimestamp: 2019-01-19T08:51:08Z
    generation: 2
    name: example-mysql
    namespace: default
    resourceVersion: "7707636"
    selfLink: /apis/lemonmysql.io/v1alpha1/namespaces/default/mysqls/example-mysql
    uid: 5d3bb657-1bc7-11e9-868b-002241aac5f1
  spec:
    rootpass: hellopass
    size: 0
kind: List
metadata:
  resourceVersion: ""
  selfLink: ""

$ kubectl -n $NAMESPACE get pods
NAME                             READY   STATUS    RESTARTS   AGE
example-mysql-pod                1/1     Running   0          21s
mysql-operator-bf565cf4f-9sp2j   1/1     Running   0          5m1s
