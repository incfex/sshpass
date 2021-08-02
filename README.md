# sshpass with TOTP support

For ssh servers with 2FA, with a normal password and time-based one time password.

## Usage
---
### MacOS
Use `security` to store and access the password and TOTP secret into `keychain`.

```shell=1
security add-generic-password -a $LOGNAME -s "pass" -w
security add-generic-password -a $LOGNAME -s "totp" -w
```

When using sshpass, you can use `-C` or `-c` to invoke `security` command to input the password and TOTP token.

```shell=1
sshpass \
  -C "security find-generic-password -w -a $LOGNAME -s pass" \
  -c "security find-generic-password -w -a $LOGNAME -s totp \
  | oathtool -b --totp -" \
  ssh $LOGNAME@example.org
```

You can use -v parameter if something wrong.

### Linux
Similar to MacOS, you could choose to use [pass](https://www.passwordstore.org) as your password manager.

---

### 2FA bastion server and beyond servers

You can use sshpass and ssh as a proxy command for connecting beyond severs. Edit your `~/.ssh/config`:

```
Host beyond
  ProxyCommand sshpass -f ~/.ssh/pw -c ~/.ssh/totp ssh bastion -qW %h:%p
```

Then run ssh.

```
ssh beyond
```


## Parameters

Added parameters:

```
-o OTP        One time password
-C command    Command for printing password
-c command    executable file name printing TOTP token
-O OTP prompt Which string should sshpass search for the one time password prompt
```
-O option's default is `Verification code:`.

## Build

```
./bootstrap
./configure
make
```

You might need installing autoconf and automake.

## Fork from

This is a fork from the sourceforge project "sshpass".

https://sourceforge.net/projects/sshpass/

I used git-svn to create "sourceforge" branch in my github repository.

## Reference
https://zhuanlan.zhihu.com/p/362783435

https://www.nongnu.org/oath-toolkit/oathtool.1.html

https://www.passwordstore.org