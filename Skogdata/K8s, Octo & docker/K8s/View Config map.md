1. Get the deployment
	1. `kubectl get deployment <deployment-name> -n <namespace> -o yaml`
2. find the config map reference under 
	 - containers:
	      - envFrom:
	        - configMapRef:
	            name: vsys-integration-viol3
3. read the config map:
	1. `kubectl get configmap my-configmap -n <namespace> -o yaml`

