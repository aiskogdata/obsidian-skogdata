[Assigning Pods to Nodes | Kubernetes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#pod-topology-spread-constraints)

<details>
<summary>More to look into</summary>
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-isolation-restriction
https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/
https://blog.kubecost.com/blog/kubernetes-node-affinity/#anti-affinity-vs-taints-and-tolerations
</details>

K8s have two types of affinity, **node affinity** and **pod affinity**. A node is a separated chunk of resources that can act independently from each other, and do work,  or in the case of the master node, provide instructions and facilitate communication between nodes.

Pods are effectively the same type of resource segmentation where each pod instead contains the actual containers that does work. The pod is the one responsible for requesting resources from the node on behalf of the container, using its preferred and max stated resource limits. 
Additionally each pod having shared resources also means they share the same local network and effectively act as if they live on the same machine. This can help facilitate more secure communication and faster communication, as well as generating files for each other in the same file system.

**Pod affinity** is primarily used to ensure that containers that have very interconnected workloads can exist on the same machine or to ensure that two types of workloads never run together (say a super sensitive thing with secret data on it and the container that exposes an API publicly).  Effectively its just a way to tell K8s whether its allowed to run something on the same 'machine' or not. 





Pods can have *none*,*preferred*, and *restricted* affinity, telling the pod it can only run specific types/tagged nodes, prioritize running specific types of nodes, but not reject them the orchestrator thinks its needed, or have no restrictions (default).

Effectively **pod affinity** is used to have pods cluster together on the same node to allow more direct access to each other (aka fewer steps and less latency to talk together). 

you can achieve simple constraint and control using the [nodeSelector]([Assigning Pods to Nodes | Kubernetes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector)) in the pod specification. For example you can say that you want you EventHub pod to always attempt to cluster on a node that has the MessageBox pod. To do that you would set a nodeSelector tag for whatever tag(s) the MessageBox pod uses. 
 
*affinity* and *anti-affinity* are both essentially more expressive means to achieve more detailed control. It still works on labels, just with more detailed functionality.

Similarly you can use and *anti affinity* against unique tags the EventHub pod has to avoid having more than one per node. 
If this is done with the *preferred* mode the orchestrator will try to place two given instances in separate nodes that have the MessageBox pod. If there is only one MessageBox, it will place the second instance in a different node without a MessageBox pod. If there are no other nodes available (because of resource limits or other affinity restrictions for example), it will place the second Eventhub pod, in the same node as the first one, with the MessageBox.
*Restricted* is stricter still and does not allow a pod to be started in a pod with a given tag.

This all informs the Pod topology spread and start buckling against  the relevant constraints  see TopologyKey.

example file for scheduling a pod to go in a node with S1 in its security key label
```yaml
spec: affinity:
	podAffinity:
		 requiredDuringSchedulingIgnoredDuringExecution:
		  - labelSelector: matchExpressions:
		    key: security
		    operator: In
			values:
		    S1 
		    topologyKey: failure-domain.beta.kubernetes.io/zone
containers:
- name: with-pod-affinity
  image: docker.io/ocpqe/hello-pod
```
replace `podAffinity` with `podAntiAffinity` to make it avoid that node
replace `requiredDuringSchedulingIgnoredDuringExecution` with `preferredDuringSchedulingIgnoredDuringExecution` to make it a preference instead of a requirement.

the topology key is used to determine relative placement of the pod after the label selector has identified the right node. `podAffinity` will *always* schedule in the same domain/topology key as the identified pod, but with *anti-affinity* it may be placed elsewhere relative to the pod according to the [Well-Known Labels, Annotations and Taints | Kubernetes](https://kubernetes.io/docs/reference/labels-annotations-taints/)
in summary it can look for names, zones, components, or if a pod is part of a certain hierarchy.  its effectively a tag to determine how "far away" the pod wants to be.

**matchExpressions** has three sub modes, *.key*, *.operator*, and *.values* (which takes an array parameter) mean it will check if a label has a given key, whether the key is set to *NotIn*, *Exists* or *DoesNotExist* when in relation got the value. or if the label contains  a set of values 
**matchLabels** is an alternative that take a map of keyvalue pairs and checks ithey match the provided labels. Effectively just loops through and check if "keyA" is equal to "ValueA" and so on. this operates as an AND operation, no OR operations possible.
**matchExpressions** also requires *all* expressions to match. To match *any* expression, use **nodeSelectorTerms**.

pod anti-affinity differs from taints and tolerations by matching on pod labels instead of being applied on the node as a whole and matching tolerations from the pods manifest. Aka  taint is the *node telling the pod* if its allowed in, while affinity and anti affinity is *the pod telling the node whether it wants to be there*.
1. Use taints on the reserved nodes and tolerations on the Elastic Search pods (this prevents any other application from placing pods on your reserved nodes).
2. Use node affinity to assign or attract Elastic Search pods to those nodes (toleration along won’t assign those pods to the nodes so you must use affinity)
3. Use pod anti-affinity to avoid having more than one Elastic Search pod on each node

affinity rules can also be defined with the scheduler configuration under the *args* section: 
```
apiVersion: kubescheduler.config.k8s.io/v1beta1
kind: KubeSchedulerConfiguration

profiles:
  - schedulerName: default-scheduler
  - schedulerName: web-scheduler
    pluginConfig:
      - name: NodeAffinity
        args:
          addedAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: name
                  operator: In
                  values:
                  - web
```


For SDs del så kan pod affinity bli brukt i hovdesak til å optimialisere. For eksemple kjører prometheus og metrics workerne i utv på forskjellige nodes, med pod affinity kan de automatisk bli plassert i samme node for å redusere latency.

Evnt også plasere alle IM workerne å samme noden så de har bedre tilgang til hverandre og  enklere å se ressursbruk for IM spesifikt. Dette er nok mindre ønskelig da det potensielt overkjører k8s sin ressurs effektivisering.
 Evnt mulig å gjøre noe lignende men fokusere på å samle alle messaging enhetene sammen, etc
 eller alle vh klientene på en. Dette er nok mer ønskelig om det er nødvendig fra et sikkerhetsperspektiv (aka at man kun har lyst at en node skal ha tilgang til VH databasen, eller setter opp spesiell monitoring eller lignende).