## go-sh
[![wercker status](https://app.wercker.com/status/009acbd4f00ccc6de7e2554e12a50d84/s "wercker status")](https://app.wercker.com/project/bykey/009acbd4f00ccc6de7e2554e12a50d84)
[![Go Walker](http://gowalker.org/api/v1/badge)](http://gowalker.org/github.com/codeskyblue/go-sh)

*If you depend on the old api, see tag: v.0.1*

install: `go get github.com/codeskyblue/go-sh`

Pipe Example:

	package main

	import "github.com/codeskyblue/go-sh"

	func main() {
		sh.Command("echo", "hello\tworld").Command("cut", "-f2").Run()
	}

Because I like os/exec, `go-sh` is very much modelled after it. However, `go-sh` provides a better experience.

These are some of its features:

* keep the variable environment (e.g. export)
* alias support (e.g. alias in shell)
* remember current dir
* pipe command
* shell build-in commands echo & test
* timeout support

Examples are important:

	sh: echo hello
	go: sh.Command("echo", "hello").Run()

	sh: export BUILD_ID=123
	go: s = sh.NewSession().SetEnv("BUILD_ID", "123")

	sh: alias ll='ls -l'
	go: s = sh.NewSession().Alias('ll', 'ls', '-l')

	sh: (cd /; pwd)
	go: sh.Command("pwd", sh.Dir("/")).Run()

	sh: test -d data || mkdir data
	go: if ! sh.Test("dir", "data") { sh.Command("mkdir", "data").Run() }

	sh: cat first second | awk '{print $1}'
	go: sh.Command("cat", "first", "second").Command("awk", "{print $1}").Run()

	sh: count=$(echo "one two three" | wc -w)
	go: count, err := sh.Echo("one two three").Command("wc", "-w").Output()

	sh(in ubuntu): timeout 1s sleep 3
	go: c := sh.Command("sleep", "3"); c.Start(); c.WaitTimeout(time.Second) # default SIGKILL
	go: out, err := sh.Command("sleep", "3").SetTimeout(time.Second).Output() # set session timeout and get output)

	sh: echo hello | cat
	go: out, err := sh.Command("cat").SetInput("hello").Output()

	sh: cat # read from stdin
	go: out, err := sh.Command("cat").SetStdin(os.Stdin).Output()

If you need to keep env and dir, it is better to create a session

	session := sh.NewSession()
	session.SetEnv("BUILD_ID", "123")
	session.SetDir("/")
	# then call cmd
	session.Command("echo", "hello").Run()
	# set ShowCMD to true for easily debug
	session.ShowCMD = true

for more information, it better to see docs.
[![Go Walker](http://gowalker.org/api/v1/badge)](http://gowalker.org/github.com/codeskyblue/go-sh)

### contribute
If you love this project, starring it will encourage the coder. Pull requests are welcome.

support the author: [alipay](https://me.alipay.com/goskyblue)

### thanks
this project is based on <http://github.com/codegangsta/inject>. thanks for the author.

# the reason to use Go shell
Sometimes we need to write shell scripts, but shell scripts are not good at working cross platform,  Go, on the other hand, is good at that. Is there a good way to use Go to write shell like scripts? Using go-sh we can do this now.
