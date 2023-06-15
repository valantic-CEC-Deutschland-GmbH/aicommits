# aicommit
Uses `git diff --staged` to generate better commit messages.

# 
 - `composer global require valantic/aicommits`
 - Retrieve your [OpenAI API Key](https://platform.openai.com/account/api-keys).)
 - Bash Example: adjust or create your `.bashrc-personal`

```shell
export OPENAI_KEY=sk-...

#  Commit everything
function commit() {
  commitMessage="$*"
  git add .
    
  if [ "$commitMessage" = "" ]; then
     commitMessage = (php aicommits)
  fi
  
  echo "git commit -a -m '${commitMessage}'"
}

```

# assumtions gitlab repo configuration
 - JIRA connected
 - configured git hook following a naming convention i.e. `task/jiraticket-xxxx_branch_description` (`#((feature|bugfix|task|hotfix|improvement|release)/jiraticket-[0-9]{1,})|master|staging|no-task/(.+)?$#i`) 

# usage
 - php aicommits.php


# ToDo
## perfect code, no todos, no bugs ;-)