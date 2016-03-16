# bashrc-setting:

## My bash-prompt
```sh
PS1='--- \u in \w (`ls -A1 -p | wc -l`) ---\n>>> '
```
this outputs something like:
```
--- jrudolph in ~/Projects/Stuff (75) ---
>>> 
```

## Navigation-aliase:
```sh
alias cd1='cd ..'
alias cd2='cd ../..'
alias cd3='cd ../../..'
alias cd4='cd ../../../..'
alias cdlast='cd -'
```

## Listing files-aliase
```sh
alias lsl='ls -l'
alias lsla='ls -l -a'
```

## Calculation-aliase
```sh
alias bc='bc -l'
alias calc='bc'
```

# clipboard aliase:
```
alias setclip='xclip -selection c'
alias getclip='xclip -selection clipboard -o'
```
