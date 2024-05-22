```bash
#depends on azure-cli

#depends on jq

  

#arg 1 = registry

#arg 2 = nr of days

  

#apt-get install azure-cli

#apt-get install jq

  

#!/bin/bash

az acr login -n sddeletetest

#om az login trenges bruk deete isteden. men dette antar at shellet er validert allerede gjennom az login eller ved å kjøres i azure shell eller tilsvarende

#ACCESS_TOKEN=$(az acr login --name sddeletetest --expose-token --output json --query accessToken)

#echo "$ACCESS_TOKEN"

#az acr login sddeletetest.azurecr.io --username 00000000-0000-0000-0000-000000000000 --password/stdin << $ACCESS_TOKEN

  

ACR_NAME="$1"

  

# Get a list of all repository names in the Azure Container Registry

REPO_LIST=$(az acr repository list -n $ACR_NAME -o tsv)

  

UTC_NOW=$(date -u '+%Y-%m-%dT%H:%M:%SZ')

UTC_PAST=$(date -d "$UTC_NOW -$2 days" -u '+%Y-%m-%dT%H:%M:%SZ')

  

# Loop through each repository to list image manifests and delete if matching regex and age

for REPO_NAME in $REPO_LIST; do

    IMAGE_LIST=$(az acr repository show-tags --name $ACR_NAME --repository $REPO_NAME --orderby time_desc --detail -o json)

  

    for IMAGE in $(echo "$IMAGE_LIST" | jq -c '.[]'); do

        IMAGE_TAG=$(echo "$IMAGE" | jq -r '.name')

        IMAGE_DATE=$(echo "$IMAGE" | jq -r '.lastUpdateTime')

        IMAGE_DIGEST=$(echo "$IMAGE" | jq -r '.digest')

  

        PARSED_DATE=$(date -d "${IMAGE_DATE}" '+%Y-%m-%d %H:%M:%S')

  

        if [[ $PARSED_DATE > $UTC_PAST ]];

        then

            continue

        fi

            echo "$PARSED_DATE is older than $UTC_PAST"

  

        # Check if IMAGE_TAG matches the regex pattern

        if [[ $IMAGE_TAG =~ ([[:digit:]]+\.[[:digit:]]+\.[[:digit:]]+)- ]]; then

            az acr repository untag -n $ACR_NAME --image "$REPO_NAME":"$IMAGE_TAG"

        fi

    done

done

  

REPO_LIST=$(az acr repository list -n $ACR_NAME -o tsv)

PURGE_CMD="az acr purge --ago 1m --untagged"

for REPO in $REPO_LIST; do

    REPO_NAME=$REPO | tr -d '\r'

    FILTER="'$REPO_NAME:.'"

    CMD_CONSTRUCT="--filter $FILTER"

    PURGE_CMD="$PURGE_CMD $CMD_CONSTRUCT"

done

  

#creates dummy docker template needed to run command

touch acb.yaml

#deletes all the untagged images without deleting tags related in manifest. If this is run, the tags disappear but the space used is not freed

az acr run --cmd "$PURGE_CMD" --registry sddeletetest .
```