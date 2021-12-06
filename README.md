# First golang program

## Setup

### Install [goenv](https://github.com/syndbg/goenv)

```
# https://github.com/syndbg/goenv/blob/master/INSTALL.md#installation
git clone https://github.com/syndbg/goenv.git ~/.goenv

# For bash
echo 'export GOENV_ROOT="$HOME/.goenv"' >> ~/.bash_profile
echo 'export PATH="$GOENV_ROOT/bin:$PATH"' >> ~/.bash_profile

# For ZSH
echo 'export GOENV_ROOT="$HOME/.goenv"' >> ~/.zshenv
echo 'export PATH="$GOENV_ROOT/bin:$PATH"' >> ~/.zshenv
```

### Set Golang environment

```
# For bash
echo 'eval "$(goenv init -)"' >> ~/.bash_profile
echo 'export PATH=$GOROOT/bin:$PATH' >> ~/.bash_profile
echo 'export GOPATH=$HOME/go' >> ~/.bash_profile
echo 'export PATH=$GOPATH/bin:$PATH' >> ~/.bash_profile

# For ZSH
echo 'eval "$(goenv init -)"' >> ~/.zshenv
echo 'export PATH=$GOROOT/bin:$PATH' >> ~/.zshenv
echo 'export GOPATH=$HOME/go' >> ~/.zshenv
echo 'export PATH=$GOPATH/bin:$PATH' >> ~/.zshenv
```

`GOROOT` is your golang installation folder

`GOPATH` is your golang workspace location (equals to `$(go env GOPATH)` by default)

A [workspace](https://go.dev/doc/gopath_code#Workspaces) is a directory hierarchy with two directories at its root: `src` contains Go source files, and `bin` contains executable commands.

### Install a golang version

```
# List available versions
goenv install -l

# Install a specific version
goenv install <version>

# Check install dir
goenv prefix

# Check go is installed
go version
```

### Set up VSCode

- install [golang.go extension](https://marketplace.visualstudio.com/items?itemName=golang.Go))
- disable gopls to have autocompletion (https://github.com/microsoft/vscode-go/issues/3086#issuecomment-601498855)

## Init workspace

https://go.dev/doc/gopath_code
https://go.dev/doc/code

```
mkdir $GOPATH/src/hello
cd $GOPATH/src/hello

cat <<EOF > hello.go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world.")
}
EOF
```

## Build code

```
go mod init hello
go mod tidy
go build
```

A `hello` go executable is generated in the current directory.

## Test your program

```
./hello
```

## Install your program

```
go install
```

A go executable is generated and installed to the workspace's (defined by $GOPATH) `bin` directory.

```
ls $GOPATH/bin/hello
```

## Launch your program

If you have built it :
```
./hello
```

If your have installed it :
```
$GOPATH/bin/hello
```

Note: if you have added `$GOPATH/bin` to your `$PATH`, you can launch the command anywhere just by executing `hello`.

## Import a local library

- Create a package :

```
mkdir greet

cat <<EOF >> ./greet/init.go
package greet

// Private visibility
var test1 = "private variable"

// Public visibility
var Test2 = "public variable"

// Private visiblity
func greet() string {
	return "hello"
}

// Public visibility
func Greet2() string {
	return "Greeting !"
}
EOF
```

- Check that the package compiles

```
go build ./greet
```

As this is not a main package, this does not produce any output, serving only as a check that the package can be built.
https://pkg.go.dev/cmd/go#hdr-Compile_packages_and_dependencies

- Import the package in your main and use it by calling the  exported function (capitalized identifier name) :

```
cat <<EOF > hello.go
package main

import (
	"fmt"
	greet "hello/greet"
)

func main() {
	fmt.Println("Hello, world.")
	fmt.Println(greet.Greet2())
}
EOF
```

- Test your new code :

```
go build
./hello
```
