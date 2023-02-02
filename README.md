# brief_7 démarche

- Creation de mon resource groupe
az group create --location francecentral --name ryanb7

- Creation Cluster Aks + génération clé SSH
az aks create -g ryanb7 -n AKSClusterRyan --enable-managed-identity --node-count 2 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys


- Connection du cluster AKS et Azure
az aks get-credentials --resource-group ryanb7 --name AKSClusterRyan

- Création secret Redis
kubectl create secret generic redis-secret-ryan --from-literal=username=devuser --from-literal=password=password_19ryan
