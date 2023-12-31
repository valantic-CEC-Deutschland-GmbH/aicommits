# aicommit
Uses `git diff --staged` to generate better commit messages.

# demo (changes in two different repositories)
![Peek 2023-06-15 19-29.gif](Peek%202023-06-15%2019-29.gif)

# Setup
 - `composer global require valantic-cec/aicommits`
 - Add `OPENAI_KEY` to your environment and create your own AI Git Commit command ([OpenAI API Key](https://platform.openai.com/account/api-keys))
   - Example for `bash`: adjust or create your `.bashrc/.zshrc/.bashrc-personal`

```shell
# Basic example (~/.bashrc or ~/.bash-personal)

# set global composer directory into path
if [ -d "$HOME/.config/composer/vendor/bin" ] ;
  then PATH="$HOME/.config/composer/vendor/bin:$PATH"
fi

# set OPENAI_KEY environment variable
export OPENAI_KEY=sk-...

# Commit everything helper function
function commit() {
  commitMessage="$*"

  git add .

  if [ "$commitMessage" = "" ]; then
    commitMessage=$(aicommits)
  fi

  # combine branchname and aicommits
  eval "git commit -a -m '${commitMessage}'"
}
```

```shell
# Extended example following naming conventions  (~/.bashrc or ~/.bash-personal)

# set global composer directory into path
if [ -d "$HOME/.config/composer/vendor/bin" ] ;
  then PATH="$HOME/.config/composer/vendor/bin:$PATH"
fi

# set OPENAI_KEY environment variable
export OPENAI_KEY=sk-...

# Commit everything helper function
function commit() {
  commitMessage="$*"

  git add .

  # Get the current branch name
  branch=$(git rev-parse --abbrev-ref HEAD)

  # Define the regular expression pattern for your branch naming convension
  # Example Branchname: feature/spry-1234_add_fancy_feature
  pattern="^(feature|bugfix|task|hotfix|improvement|release)/([[:alnum:]-]+)"

  # Extract the specific part using regex
  if [[ $branch =~ $pattern ]]; then
    extracted_part="${BASH_REMATCH[2]}"
  else
    extracted_part="no-task"
  fi

  if [ "$commitMessage" = "" ]; then
    commitMessage=$(aicommits)
  fi

  # combine branchname and aicommits
  eval "git commit -a -m '${extracted_part} ${commitMessage}'"
}
```

.zshrc example: https://github.com/valantic-CEC-Deutschland-GmbH/aicommits/blob/main/.zshrc-example

# assumtions for gitlab repo configuration
 - JIRA connected
 - configured git hook following a naming convention i.e. `task/jiraticket-xxxx_branch_description` (`#((feature|bugfix|task|hotfix|improvement|release)/jiraticket-[0-9]{1,})|master|staging|no-task/(.+)?$#i`) 

# development usage
 - php aicommits

# ToDo
## perfect code, no todos, no bugs ;-)
