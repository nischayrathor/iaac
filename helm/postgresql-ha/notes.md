#### Dependency Build

```bash
helm dependency build
```

#### Creating certs for postgres

```bash
kubectl create secret generic postgresql-tls-secret \
    --from-file=../../../task-manager/certs/server.crt \
    --from-file=../../../task-manager/certs/server.key \
    --from-file=../../../task-manager/certs/root.crt
```

#### Installing chart

```bash
helm install postgresql-ha . -f values.yaml -n postgresql
```