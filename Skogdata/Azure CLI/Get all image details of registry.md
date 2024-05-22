```bash
#!/bin/bash
$ACR_NAME="sddeletetest"

$ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query "loginServer" -o tsv)

# Get a list of all repository names in the Azure Container Registry
$REPO_LIST=$(az acr repository list -n $ACR_NAME -o tsv)

$DELETE_LIST = @()

# Loop through each repository to list image manifests and delete old images
foreach($REPO_NAME in $REPO_LIST)
{
    #echo " "

     echo "===$REPO_NAME==="

    $IMAGE_LIST = $(az acr repository show-tags --name $ACR_NAME --repository $REPO_NAME --orderby time_desc --detail -o table)

    echo $IMAGE_LIST
}```
