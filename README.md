# blah blah blah

[Did this here, check it out](https://hugo-test-site.now.sh/post/deploying-with-now-cli/)

## shell function

```sh
# Publish Hugo Site to Now
# $1 - name of deployment
# $2 - alias
publish() {
  if [ "$PWD" == $HOME ]; then
    # macOS will throw "permission denied" on rm -rf public anyways
    # but this makes it a lot less scary looking.
    echo "ERROR: Cannot call from home directory."
    return 1
  else
    # Look for 'public/'
    if [ -d "public" ]; then
      # Remove old 'public/'
      command rm -rf public
      echo "Removed old public directory."
      # Generate new 'public/'
      command hugo
      echo "Generated site!"
      # Deploy and pipe the instance url to variable.
      local url=$(now public -n=$1 --static 2>/dev/null)
      echo "Sited deployed to ${url}"
      # Alias deployment with arg and deployment $url
      command now alias $url $2
    else
      command hugo
      echo "Generated site!"
      local url=$(now public -n=$1 --static 2>/dev/null)
      echo "Sited deployed to ${url}"
      command now alias $url $2
    fi
  fi
}
```