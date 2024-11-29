```bash
#!/bin/bash

  

# Define the parent directory containing the subfolders with git repositories

PARENT_DIR=$(pwd -W)

echo "$PARENT_DIR"

  

folders=$(find $PARENT_DIR -maxdepth 1 -type d)

  

# Loop through each subfolder in the parent directory

for SUBFOLDER in $folders; do

  # Check if the subfolder is actually a directory

  if [ -d "$SUBFOLDER" ]; then

    #Check if the subfolder contains a .git directory

    if [ -d "$SUBFOLDER/.git" ]; then

      echo "Pulling latest changes in $SUBFOLDER"

      # Change to the subfolder directory

      cd "$SUBFOLDER"

      # Run the git pull command

      git pull

      # Change back to the parent directory

      cd "$PARENT_DIR"

    else

      echo "$SUBFOLDER is not a git repository"

    fi

  fi

done
```