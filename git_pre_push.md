# Git pre-push
### Execute PHPUnit
```sh
#!/usr/bin/env bash
# Install instructions (as MarkDown):
# 1. Create file .git/hooks/pre-push and make it executable
#    ```sh
#    #!/usr/bin/env bash
#    PROJECT_ROOT="../../"
#    cd "$PROJECT_ROOOT"
#    source pre-push.sh
#    ```





##### Options

# Aliases must be expanded
shopt -s expand_aliases





##### Functions and aliases

####
# Prepends a '# ' before every line of stdout
####
alias prependHash="sed 's/^/# /'"

####
# Runs PHPUnit in the src directory
#
# @return status code of the phpunit command
####
function executeUnitTests {
  cd src
  vendor/bin/phpunit
  return $?
}





##### Main

echo "### START pre push"

echo "## PHPUnit"
executeUnitTests | prependHash
unitTestResult=${PIPESTATUS[0]}





##### Exit

prePushResult=${unitTestResult}

if (( $prePushResult == 0 )); then
    successString="SUCCESS"
else
    successString="FAILED"
fi

echo "### END pre push. Result: ${successString}"
exit ${prePushResult}
```
