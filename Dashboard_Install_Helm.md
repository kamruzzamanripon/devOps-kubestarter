## Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

##  Add the Kubernetes Dashboard repo using Helm.
```
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm repo list

```
![image](https://github.com/user-attachments/assets/d3b2bf25-a1b4-49db-aadb-04dee8ad77d9)

## Install Kubernetes Dashboard Using Helm
```
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard

```

## Output:
```
*************************************************************************************************
*** PLEASE BE PATIENT: Kubernetes Dashboard may need a few minutes to get up and become ready ***
*************************************************************************************************

Congratulations! You have just installed Kubernetes Dashboard in your cluster.

To access Dashboard run:
  kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443

NOTE: In case port-forward command does not work, make sure that kong service name is correct.
      Check the services in Kubernetes Dashboard namespace using:
        kubectl -n kubernetes-dashboard get svc

Dashboard will be available at:
  https://localhost:8443
```

## Once Kubernetes Dashboard is installed, you can verify it using the following command:
```
kubectl get svc -n kubernetes-dashboard
```

## Output
![image](https://github.com/user-attachments/assets/392d0314-fad2-46c1-a672-04f843699735)

## Generate Token for Kubernetes Dashboard
```
nano k8s-dashboard-account.yaml

```
## Add the following content:
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```
##  apply the above configuration to the Kubernetes cluster.
```
kubectl create -f k8s-dashboard-account.yaml
```

## generate a token using the following command:
```
kubectl -n kube-system create token admin-user
```

## Access Kubernetes Dashboard
```
kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard-kong-proxy 10443:443 --address 0.0.0.0 &
```
## You will see the following output:
```
Forwarding from 0.0.0.0:10443 -> 8443
```
## https://server-ip:10443. You will be asked to enter a token to log in.
![image](https://github.com/user-attachments/assets/71d1042c-5f31-47a6-9056-44e645b71cbb)



