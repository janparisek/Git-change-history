# Git-change-history

Changes the author info of **ALL** past commits. This changes past commit history and forces forked projects to fetch the entire repository. It should only be used as a last resort.

# Procedure

1. Open Git Bash.

1. Create a fresh, bare clone of your repository:

   ```bash
   git clone --bare https://github.com/user/repo.git
   cd repo.git
   ```

1. Copy and paste the script, replacing the following variables based on the information you gathered:
   - OLD_EMAIL
   - CORRECT_NAME
   - CORRECT_EMAIL

   ```bash
   #!/bin/sh
   
   git filter-branch --env-filter '
   
   OLD_EMAIL="your-old-email@example.com"
   CORRECT_NAME="Your Correct Name"
   CORRECT_EMAIL="your-correct-email@example.com"
   
   if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
   then
   export GIT_COMMITTER_NAME="$CORRECT_NAME"
   export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
   fi
   if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
   then
   export GIT_AUTHOR_NAME="$CORRECT_NAME"
   export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
   fi
   ' --tag-name-filter cat -- --branches --tags
   ```
1. Press Enter to run the script.

1. Review the new Git history for errors.

1. Push the corrected history to GitHub:

   ```bash
   git push --force --tags origin 'refs/heads/*'
   ```

1. Clean up the temporary clone:

   ```bash
   cd ..
   rm -rf repo.git
   ```
