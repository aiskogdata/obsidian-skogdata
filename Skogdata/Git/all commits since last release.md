```sh
#!/bin/bash

# Get the latest release branch (assuming naming pattern "release/*")
LATEST_RELEASE_BRANCH=$(git branch -r --sort=-committerdate | grep -E 'origin/Release/' | head -n 1 | sed 's/origin\///'| tr -d '[:space:]')

if [ -z "$LATEST_RELEASE_BRANCH" ]; then
  echo "No release branch found."
fi

OUTPUT_FILE="commits_since_${LATEST_RELEASE_BRANCH}.csv"

echo "Latest release branch: $LATEST_RELEASE_BRANCH"
# Find the merge base between master and the latest release branch
MERGE_BASE=$(git merge-base master "origin/$LATEST_RELEASE_BRANCH")

if [ -z "$MERGE_BASE" ]; then
  echo "Could not determine merge base."
fi

echo "Merge base commit: $MERGE_BASE"

# List commits on master since the last change on the latest release branch
echo "Commits on master since $MERGE_BASE:"

COMMIT_LOG=$(git log --oneline "$MERGE_BASE"..master)
printf "%-12s | %-8s | %s\n" "Commit Hash" "PR Number" "Message"
printf -- "---------------------------------------------\n"

# Parse each line
while IFS= read -r line; do
  # Extract commit hash (first word)
  COMMIT_HASH=$(echo "$line" | awk '{print $1}')
  
  # Extract PR number (number between 'PR' and ':')
  PR_NUMBER=$(echo "$line" | grep -oP 'PR \K[0-9]+')

  # Extract commit message (everything after ':')
  MESSAGE=$(echo "$line" | sed -E 's/^[^:]+: //')

  # Print formatted row
  printf "%-12s | %-8s | %s\n" "$COMMIT_HASH" "$PR_NUMBER" "$MESSAGE"

  # Append to CSV file 
  echo "\"$COMMIT_HASH\",\"$PR_NUMBER\",\"$MESSAGE\"" >> "$OUTPUT_FILE" 
  
done <<< "$COMMIT_LOG"
echo "Exported commit log to $OUTPUT_FILE"
```