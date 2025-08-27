# IRC
https://modern.ircdocs.horse/dcc  
```
DCC <type> <argument> <host> <port>

# Convert network byte order address
python3
import ipaddress
ipaddress.IPv4Address(<Byte Order>)
```

# PGP/GPG
```
# Decrypt files with private key
gpg --import <Private Key>
gpg --list-secret-keys
gpg --decrypt <Encrypted Message> --output <File>
```

# Wireshark
```

```

# Cracking hash
```
john hash --incremental=digits --max-length=7
```

# Python
```python
# Decompile python compiled byte code
pip3 install uncompyle6
uncompyle6 -o . <.pyc File>

# Byte-compiled Python module for CPython 3.9 (magic: 3425)
# Will not run unless using the same python version and must have the .pyc extension (even in Linux)
# RuntimeError: Bad magic number in .pyc file
```
