# README

## Steps
### Spin-up OpenShift GitOps Instance

- https://github.com/redhat-developer/gitops-operator


#### Operator Installation

- Operator Subscription also creates openshift-gitops namespace and an ArgoCD instance.

```sh
oc apply -k bootstrap/openshift-gitops/operator
```

#### Instance Configuration

1) To allow Argo CD to manage resources in other namespaces apart from where it is installed, configure the target namespace with a argocd.argoproj.io/managed-by label.

```sh
oc label namespace <namespace> argocd.argoproj.io/managed-by=<instance_name> 
```

2) To allow Argo CD to manage OpenShift cluster resources (operators.coreos.com, user.openshift.io, rbac.authorization.k8s.io, config.openshift.io, storage.k8s.io, console.openshift.io) modify subscription with ARGOCD_CLUSTER_CONFIG_NAMESPACES environment variable. Argo CD is not granted cluster-admin.



oc apply -k bootstrap/openshift-gitops/instance --server-side=true



#### Create app-of-apps

```sh
oc apply -f bootstrap/app-of-apps/
```



### API 

```sh

ssh centos-streams-root "openssl s_client -showcerts -connect api.ocp4.ypolat.me:6443 2>/dev/null </dev/null |  sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ca.cert"
scp centos-streams-root:~/ca.cert /Users/ypolat/Documents/my-cluster/credentials/ca.cert
```

### Vault

- https://medium.com/hybrid-cloud-engineering/vault-integration-into-openshift-container-platform-b57c175a79da