## basic pod management
### restart

#### Method 1
`kubectl get pod <pod_name> -n <namespace> -o yaml | kubectl replace --force -f -`
feks:
`kubectl get pod vsys-im-messaging-sender-759f4b8bbc-62hhn -o yaml | kubectl replace --force -f -`

bruk `kubectl get pods -l Vsys.AppGroup=Vsys.Im` for å finne fra label

#### Method 2
evnt `kubectl scale deployment deployments-76150 --replicas=0`
og så `kubectl scale deployment deployments-76150 --replicas=1`

#### Method 3
`kubectl rollout restart deployment deployments-76150`
tar ned en og en pod i et replica set til alle er nye

#### Method 4
`kubectl delete pod vsys-im-messaging-sender-759f4b8bbc-62hhn` 
sletter en pod, og siden k8s har et krav om minst en pod, startes en nye en


#### Get worker with creation and end timestamp
`kubectl get jobs -l Vsys.Application=Vsys.Im.ControlMeasurement.Recalculation -o custom-columns=TIMESTAMP:.metadata.creationTimestamp,NAME:.metadata.name,COMPLETIONTIME:.status.completionTime`