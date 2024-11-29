```shell
kubectl scale deployment --selector $(get_octopusvariable "Label-AppGroup")=$(get_octopusvariable "Label-AppGroup-Value"),"Vsys.Application"="Vsys.Im.ControlMeasurement.Messaging.Receiver"  --replicas 1
```
