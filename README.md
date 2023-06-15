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


# usage
 - php aicommits.php


# ToDo
## perfect code, with no bugs ;-)