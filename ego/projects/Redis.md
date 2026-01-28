
*PS1* -   prompt string 1

common PS1 escape sequences
- \u -   user name
- \h -   host name
- \w -  current working directory

`\u@\h:\w$`

`PS1 = "\w $(git branch --show-current)\$"`
execute commands and include their output in the prompt using `$(command)` 

