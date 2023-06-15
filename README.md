# aicommit
Uses `git diff --staged` to generate better commit messages.

# 
 - `composer global require valantic/aicommits`
 - publish your openai key as a environment variable `OPENAI_KEY=sk-....` (You can retrieve it from [OpenAI API Keys page](https://platform.openai.com/account/api-keys).)
 - add i.e. a helper function to your `~/.bashrc`
```shell
#  Commit everything
function commit() {
  commitMessage="$*"

  if [ "$commitMessage" = "" ]; then
     commitMessage = (php aicommits)
  fi
    
  echo "git commit -a -m '${commitMessage}'"
}
```


# usage
 - OPENAI_KEY=sk-.... php aicommits.php


# ToDo
## perfect code, with no bugs ;-)