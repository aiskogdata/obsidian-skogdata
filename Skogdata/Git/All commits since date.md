```sh
#!/bin/bash

# Ensure a date argument is provided
if [ -z "$1" ]; then
  echo "Usage: $0 <YYYY-MM-DD>"
  exit 1
fi  

START_DATE="$1"
OUTPUT_FILE="commits_since_${START_DATE}.csv" 

# Store commit log in a variable (since the given date)
COMMIT_LOG=$(git log --since="$START_DATE" --oneline master)

# Check if there are commits
if [ -z "$COMMIT_LOG" ]; then
  echo "No commits found on master since $START_DATE."
  exit 0
fi 

# Print table header
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