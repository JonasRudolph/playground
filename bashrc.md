# bashrc-setting:
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

## Enable recusive file expansion like `**/*.*`
```sh
shopt -s globstar
```
