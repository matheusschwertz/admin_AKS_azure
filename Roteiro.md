# Roteiro
 
## Checklist
https://github.com/Azure/review-checklists

## AKS Release Page
- https://releases.aks.azure.com/

## Updates
https://learn.microsoft.com/en-us/azure/architecture/operator-guides/aks/aks-upgrade-practices
https://learn.microsoft.com/en-us/azure/aks/upgrade

## Check CLuster update
 az aks show -n $aksname -g $rgname -o json --query "{name:name,k8sv:currentKubernetesVersion,upgradeSettings:upgradeSettings,poolname:agentPoolProfiles[].name,nodeImage:agentPoolProfiles[].nodeImageVersion,autoUpgradeProfile:autoUpgradeProfile}"

## Check Region update
az aks nodepool get-upgrades --name nodepool1 --cluster-name $aksname -g $rgname -o json --query latestNodeImageVersion

## Manutencao - Check
az aks maintenanceconfiguration list --cluster-name $aksname --resource-group $rgname
## Manutencao - Criar
az aks maintenanceconfiguration add -g $rgname --cluster-name $aksname --name aksManagedNodeOSUpgradeSchedule --schedule-type Weekly --day-of-week Saturday --interval-weeks 1 --duration 8 --utc-offset=-03:00 --start-time 00:00

## Kured
https://learn.microsoft.com/en-us/azure/aks/node-updates-kured#deploy-kured-in-an-aks-cluster
https://github.com/kubereboot/charts/tree/main/charts/kured
https://kured.dev/docs/configuration/

## PDB
https://kubernetes.io/docs/tasks/run-application/configure-pdb/
kf store-front-pdb.yaml
k drain aks-nodepool1-50780131-vmss000001 --ignore-daemonsets --delete-emptydir-data

## Topology Label
https://kubernetes.io/docs/reference/labels-annotations-taints/#topologykubernetesiozone
https://learn.microsoft.com/en-us/azure/aks/availability-zones

## Uptime SLA
https://learn.microsoft.com/en-us/azure/aks/free-standard-pricing-tiers
az aks show -n $aksname -g $rgname -o json --query sku

## Storage Class
https://learn.microsoft.com/en-us/azure/aks/csi-storage-drivers
az aks show -n $aksname -g $rgname -o json --query storageProfile

https://learn.microsoft.com/en-us/azure/aks/azure-files-csi
https://learn.microsoft.com/en-us/azure/aks/azure-csi-files-storage-provision#dynamically-provision-a-volume 
https://kubernetes.io/docs/concepts/storage/storage-classes/

kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/pvc-azurefile-csi.yaml

## Taints/Toleration or Label
https://learn.microsoft.com/en-us/azure/aks/operator-best-practices-advanced-scheduler
https://learn.microsoft.com/en-us/azure/aks/use-labels

- Tain
kubectl taint nodes aks-nodepool1-32428415-vmss000003 key1=value1:NoSchedule
kg no aks-nodepool1-32428415-vmss000003 -o jsonpath={.spec.taints[*]}

az aks nodepool update --cluster-name $aksname -g $rgname -n nodepool2 --node-taints key2=value2:NoSchedule

- Label
kubectl label nodes aks-nodepool1-32428415-vmss000003 demolabel=1
kg no -L demolabel



## Add-ons
https://learn.microsoft.com/en-us/azure/aks/integrations

https://learn.microsoft.com/en-us/azure/aks/keda-about

az aks update --resource-group $rgname --name $aksname --enable-keda 
