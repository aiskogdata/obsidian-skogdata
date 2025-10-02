
Hente ut sha for latest
`curl -s -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET  https://mcr.microsoft.com/v2/dotnet/aspnet/manifests/latest | grep '"digest"' | head -n1 | awk '{print $2}' | tr -d '"'`

```sh
#!bin/bash  

latest_sha=$(curl -s -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET  https://mcr.microsoft.com/v2/dotnet/aspnet/manifests/latest | grep '"digest"' | head -n1 | awk '{print $2}' | tr -d '"')

matching_tags=()

tags=$(curl -s https://mcr.microsoft.com/v2/dotnet/aspnet/tags/list | jq -r '.tags[]' | tr -d '\r' | grep -E '^(10|9|8)\.' | grep -Ev 'arm64|composite|arm32|amd64|distroless|preview|azurelinux|rc')

total=$(echo "$tags" | wc -l)

count=0 

for tag in $tags; do
    count=$((count + 1))

    echo "Checking tag $count of $total: $tag"  

    sha=$(curl -s -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET  https://mcr.microsoft.com/v2/dotnet/aspnet/manifests/$tag | grep '"digest"' | head -n1 | awk '{print $2}' | tr -d '"')

  tag_digest=$(curl -s -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET  https://mcr.microsoft.com/v2/dotnet/aspnet/manifests/$tag | grep '"digest"' | head -n1 | awk '{print $2}' | tr -d '"')

  if [ "$tag_digest" = "$latest_sha" ]; then

    matching_tags+=("$tag")

  fi

done

  

echo "Matching tags:"

printf '%s\n' "${matching_tags[@]}"
```