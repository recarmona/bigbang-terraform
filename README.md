# Big Bang Terraform Start

### Using a vars file:

```bash
# Copy and edit with your values
cp terraform.tfvars.example terraform.tfvars 

terraform init
terraform plan
terraform apply
```

### Using environment variables:

```bash
# Update with your values
export TF_VAR_registry_credentials='[{registry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'

#Optional: Reduce flux resource requests for edge/resource constrained environments
export TF_VAR_reduce_flux_resources=true

terraform init
terraform plan
terraform apply
```

### Using inline arguments:

```bash
# Update with your values inline
terraform init
terraform plan -var 'registry_credentials=[{registry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'
terraform apply -var 'registry_credentials=[{regsitry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'
```
# Deploying Bigbang

$ kubectl create namespace bigbang

$ gpg --export-secret-key --armor ${fp} | kubectl create secret generic sops-gpg -n bigbang --from-file=bigbangkey=/dev/stdin

$ terraform apply

After terraform apply finish 3 min later apply this (its a fix to a gatekeeper on GCP): 

```bash
$ cat << EOF | kubectl apply -f=-
apiVersion: v1
kind: ResourceQuota
metadata:
  name: gatekeeper-resource-quota
  namespace: gatekeeper-system
spec:
  hard:
    pods: 1000
  scopeSelector:
    matchExpressions:
    - operator: In
      scopeName: PriorityClass
      values:
      - system-node-critical
      - system-cluster-critical
EOF

Check that everithing is working


$ kubectl get gitrepositories -A


$ kubectl get -n bigbang kustomizations

$ kubectl get -n bigbang secrets,configmaps


$ watch kubectl get hr,po -A


