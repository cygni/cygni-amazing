# Git
Awesome git pro-tips!

## Path-specific configuration
When working as a consultant you will be exposed to many different employers and 
most likely they give you a separate e-mail account.

Which also means that when commiting code to their repositories you should use the 
provided e-mail address rather than your Cygni one. This can be achieved by using gits
`includeIf` which came with version 2.13.

Using `includeIf` you can override and complement the global configuration 
`$HOME/.gitconfig` for a folder and it's subdirectories.

### Example
Example:  
```bash
$ cat $HOME/.gitconfig
[user]
	name = Awesome Gitter
	email = awesome.gitter@cygni.se

[includeIf "gitdir:~/code/company1/"]
	path = ~/code/company1/.gitconfig

[includeIf "gitdir:~/code/company2/"]
	path = ~/code/company2/.gitconfig
```

```bash
$ cat code/company1/.gitconfig 
[user]
	email = awesome.gitter@company1.se

[core]
	sshCommand = ssh -i ~/.ssh/id_company1
```

```bash
$ cat code/company2/.gitconfig 
[user]
	email = awesome.gitter@company2.se

[core]
	sshCommand = ssh -i ~/.ssh/id_company2
```
