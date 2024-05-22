apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: vsys-im-web  
  labels:  
    '#{__label-appgroup}': '#{__label-appgroup-value}'  
    '#{__label-application}': Vsys.Im.Web  
  namespace: '#{k8s-namespace}'  
spec:  
  selector:  
    matchLabels:  
      octopusexport: OctopusExport  
  replicas: 2  
  strategy:  
    type: RollingUpdate  
  template:  
    metadata:  
      labels:  
        '#{__label-appgroup}': '#{__label-appgroup-value}'  
        '#{__label-application}': Vsys.Im.Web  
        octopusexport: OctopusExport  
      annotations:  
        prometheus.io/port: '9095'  
        prometheus.io/scrape: 'true'  
    spec:  
      containers:  
        - name: vsys-im-web  
          image: skogdata.azurecr.io/vsys.im.web  
          ports:  
            - name: metrics  
              containerPort: 9095  
            - name: http  
              containerPort: 8080  
          envFrom:  
            - secretRef:  
                name: '#{__config-name}'  
            - configMapRef:  
                name: '#{__config-name}'  
          resources:  
            requests:  
              memory: 400Mi  
            limits:  
              memory: 400Mi  
      tolerations:  
        - key: '#{__taint-loadtype}'  
          operator: '#{__taint-loadtype-operator}'  
          value: '#{__taint-loadtype-interactive}'  
          effect: '#{__taint-loadtype-effect}'  
      securityContext:  
        runAsGroup: 10000  
        runAsUser: 10000  
        runAsNonRoot: true  
      serviceAccountName: '#{__serviceaccount}'  
      affinity:  
        nodeAffinity:  
          requiredDuringSchedulingIgnoredDuringExecution:  
            nodeSelectorTerms:  
              - matchExpressions:  
                  - key: '#{__affinity-loadtype-interactive}'  
                    operator: Exists  
\

has context menu