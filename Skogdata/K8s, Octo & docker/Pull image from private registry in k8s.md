1. in the private registry generate either a user or token with a secret and copy them with you
2. in the Kubernetes cluster run `kubectl create secret docker-registry [your kubernetes name for the secret] --docker-server=[url to registry] --docker-username=[token name] --docker-password=[Token secret]`
3. in the pod definition add the field `imagePullSecrets` under the `spec`field, and enter the name of the secret you provided
