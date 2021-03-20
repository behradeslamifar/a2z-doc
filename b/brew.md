# Brew
Homebrew/Linuxbrew (or brew) is a free and open-source software package management system that simplifies the installation of software on Apple's operating system macOS as well as Linux.
* [Homebrew Homepage](https://brew.sh/)

## Install Linuxbrew on Ubuntu/Debian
For install brew on linux, we can use linuxbrew-wrapper packege. This wrapper package contains no linuxbrew itself, but just a wrapper script and an Homebrew/Linuxbrew upstream install script. And the wrapper script does:
* run linuxbrew if it exists at your home.
* run upstream install script if found no linuxbrew instance at your home.

```
apt-get update
apt-get install linuxbrew-wrapper
```

Add linuxbrew to PATH variable
```
$ vi ~/.bashrc
...
export PATH=$PATH:/home/linuxbrew/.linuxbrew/bin/:/home/linuxbrew/.linuxbrew/sbin/
```

## Work with brew command
Install new package
```
brew install <package_name>
```


