### Setting up Crossplane and AWS, GCP providers

***Install Crossplane to Kubernetes cluster.***

```bash
helm repo add \
    crossplane-stable https://charts.crossplane.io/stable

helm repo update

helm install crossplane \
    crossplane-stable/crossplane \
    --namespace crossplane-system \
    --create-namespace
```

***Installing crossplane provider for AWS***

```bash
cat <<EOF | kubectl apply -f -
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-aws
spec:
  package: xpkg.upbound.io/crossplane-contrib/provider-aws:v0.51.0
EOF

cat aws-credentials.txt
[default]
aws_access_key_id = <AWS_ACCESS_KEY>
aws_secret_access_key = <AWS_SECRET_KEY>


kubectl create secret \
    generic aws-secret \
    -n crossplane-system \
    --from-file=creds=./aws-credentials.txt

cat <<EOF | kubectl apply -f -
apiVersion: aws.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: aws-secret
      key: creds
EOF
```

***Installing crossplane provider for GCP***
```bash
cat <<EOF | kubectl apply -f -
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-gcp
spec:
  package: xpkg.upbound.io/crossplane-contrib/provider-gcp:v0.22.0
EOF

## GCP Credentials file gcp-credentials.json

kubectl create secret \
    generic gcp-secret \
    -n crossplane-system \
    --from-file=creds=./gcp-credentials.json

cat <<EOF | kubectl apply -f -
apiVersion: gcp.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  projectID: <project-id>
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: gcp-secret
      key: creds
EOF
```

#### Provisioning k8s clusters

```bash
kubectl apply -f aws-eks-cluster.yaml
kubectl apply -f gcp-gke-cluster.yaml
```

##### Links
1. https://docs.crossplane.io/latest/getting-started/provider-aws/
2. https://kind.sigs.k8s.io/
3. https://marketplace.upbound.io/