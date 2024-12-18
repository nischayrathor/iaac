### Installing helm charts

#### Deploying postgresql-ha

***Dependency Build***

```bash
helm dependency build
```

***Creating certs for postgres***

```bash
kubectl create secret generic postgresql-tls-secret \
    --from-file=../../../task-manager/certs/server.crt \
    --from-file=../../../task-manager/certs/server.key \
    --from-file=../../../task-manager/certs/root.crt

kubectl create secret generic pgpool-tls-secret \
    --from-file=../../../task-manager/certs/server.crt \
    --from-file=../../../task-manager/certs/server.key
```

***Installing chart***

```bash
helm install postgresql-ha . -f values.yaml -n postgresql
```

#### Deploying task-manager app
```bash
cd task-manager
task deploy
```

#### Links
1. https://github.com/bitnami/charts/tree/main/bitnami/postgresql-ha
2. https://taskfile.dev/usage/
