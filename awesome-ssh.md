# SSH
Awesome ssh pro-tips!


## ProxyJump
One common pattern is to use a jumpgate to log in at customer site. To simplify
loging in to the hosts on the inside you could use the option `ProxyJump` in
your `$HOME/.ssh/config`.
Note that `jumpgate.customersite.se` is excluded in the Host line.


### Example
```bash
$ cat $HOME/.ssh/config
Host *.customersite.se !jumpgate.customersite.se
    User awesomeserveracount
    ProxyJump awesomejumpaccount@jumpgate.customersite.se
```

## ProxyCommand
ProxyJump may be prohibited on the jumpgate. To make it possible to use a similar pattern you could instead use the `ProxyCommand` and `nc` on the jumphost. _NOTE_ This depends on `nc` on the jumphost. 

### Example
```bash
$ cat $HOME/.ssh/config
Host *.customersite.se !jumpgate.customersite.se
    User awesomserveraccount
    ProxyCommand ssh awesomejumpaccount@jumpgate.customersite.se nc %h %p
```

## Hosts not resolvable
If hosts you are supposed to log in to doesn't have a resolvable hostname, you
could add them to you `$HOME/.ssh/config`. This also works if you have `ProxyJump`


### Example
```bash
$ cat $HOME/.ssh/config
Host unsolvable.customersite.se
    Hostname 192.168.10.10
```


## ControlMaster
Every time you log in to a host, eiter via `ProxyJump` or directly, ssh needs
to authenticate and negotiate encryption. If you regularly log in to the same
host you could instead use `ControlMaster` and keep a long running connection
which is reused for every connection and thus reduces time to connect.

### Example
```bash
cat $HOME/.ssh/config
Host *
    ControlMaster auto
    ControlPath ~/.ssh/master-%r@%h:%p
```
