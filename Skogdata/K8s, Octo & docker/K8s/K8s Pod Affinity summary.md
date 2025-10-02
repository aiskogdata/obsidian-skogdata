Pod Affinity styrer hvilken *node* en ny-genert *pod* har lyst eller får lov å legge seg. Dette er da *poden* som forteller k8s hva den ønsker og krever, mens *taints* og *tolerations* er hvordan *noden* forteller hva den tillater å ha i seg. Taint og tolerations skal i teorien alltid vinne over pod affinity.

pod affinity har to modus *preferred* (preferredDuringSchedulingIgnoredDuringExecution) og *required* (requiredDuringSchedulingIgnoredDuringExecution).  Selv forklarende nok så vil en required pod enten kun legge seg eller aldri legge seg i en node som passer med mønsteret definert i yaml filen, men preferred vil lete etter bedre alternativer før den eventuelt aksepterer å bli lagt inn i en node som ikke passer. 

Pod affinity bruker *labelSelector* og enten *matchExpressions* (alt må passe) eller *nodeSelectorTerms* (minst en må passe)  for definere mønsteret som skal følges. 
```yaml
	podAffinity:
		 requiredDuringSchedulingIgnoredDuringExecution:
		  - labelSelector: matchExpressions:
		    key: helm.sh/chart
		    operator: In
			values:
		    ingress-nginx-4.8.3 
```
Denne leter etter en pod som har label *security* med verdien *S1*. For å få mottsatt effekt, bruker *notin*

For å gjøre dette til en *anti-affinity* regel bytters *podAffinity* ut med *podAntiAffinity*. Da blir også *topologyKey* relevant. Denne har ingen rèell effekt sammen med podAffinity, men ved bruk av podAntiAffinity, forteller denne keyen, hvor "langt unna"poden skal legge seg poden den ikke skal være i samme node som. I eksemplet under ville podAntiAffinity, forårsake at denne poden ville forsøkt å alltid ligge i en annen availability zone enn poder med labelen `helm.sh/chart : ingress-nginx-4.8.3`. 
	```
```yaml
podAntiAffinity:
	 requiredDuringSchedulingIgnoredDuringExecution:
	  - labelSelector: matchExpressions:
		key: helm.sh/chart
		operator: In
		values:
		ingress-nginx-4.8.3 
		topologyKey: failure-domain.beta.kubernetes.io/zone
```
TopologyKeys er en lang egen list.[Well-Known Labels, Annotations and Taints | Kubernetes](https://kubernetes.io/docs/reference/labels-annotations-taints/) 
De kan også defineres av cloud providers eller andre hosters, eller oss selv. 

`preferredDuringSchedulingIgnoredDuringExecution` kan også brukes for å prøve og oppnå kravet, men ikke blokkere utrulling av poden. 


