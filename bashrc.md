# bashrc-setting:

### My bash-prompt
```sh
alias current_branch='git branch 2>/dev/null | grep -E "^\*" | tr -d "* "'

function current_branch_info_or_nothing() {
    local current_branch_var=$(echo -n `current_branch`)
    if [ -z "$current_branch_var" ]; then
        echo -n ""
    else
        echo -n -e "on branch \e[35m$current_branch_var\e[m "
        current_branch_var=""
    fi
}

alias filecount='ls -A1 | wc -l'

PS1="--- \[\e[32m\]\u\[\e[m\] in \[\e[33m\]\w\[\e[m\] (\[\e[36m\]\`filecount\`\[\e[m\]) at \t \`current_branch_info_or_nothing\`---\n>>> "
```
this outputs something like:
```
--- jrudolph in ~/Projects/CrazyStuff (1) at 14:24:16 ---
>>> 
```

### Navigation-aliase:
```sh
alias cd1='cd ..'
alias cd2='cd ../..'
alias cd3='cd ../../..'
alias cd4='cd ../../../..'
alias cdlast='cd -'
```

### Listing files-aliase
```sh
alias lsl='ls -l'
alias lsla='ls -l -a'
```

### Calculation-aliase
```sh
alias bc='bc -l'
alias calc='bc'
```

### clipboard aliase:
```sh
alias setclip='xclip -selection c'
alias getclip='xclip -selection clipboard -o'
```

How to use:
```sh
>>> echo "stuff I want to paste anywhere" | setclip
>>> getclip | grep "search in text I copied with my mouse"
```

### Remind-function
```sh
############
# Expects $1 to be: an integer or a string which describes how long to sleep. E.g. '3' or '5m'
# Expects $2 to be: a string - the message
# Example: remind 3m 'Work done!' &
##
function remind () {
  sleep "$1" 2>&1 1>/dev/null
  notify-send --urgency critical 'Reminder!' 2>&1 1>/dev/null
}

```
