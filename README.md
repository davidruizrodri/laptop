Laptop
======

Laptop is a script to set up an macOS laptop for web development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

We support:
* macOS Monterey (12.3) on Apple Silicon and Intel

Older versions may work but aren't regularly tested. Bug reports for older versions are welcome.

Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/davidruizrodri/laptop/master/mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [Exuberant Ctags] for indexing files for vim tab completion
* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [The Silver Searcher] for finding things in files
* [Tmux] for saving project state and switching between projects
* [Zsh] as your shell
* [Autojump] for easily navigate directories from the command line

[Exuberant Ctags]: http://ctags.sourceforge.net/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.github.io/
[Zsh]: http://www.zsh.org/
[Autojump]: https://github.com/wting/autojump

GitHub tools:

* [Hub] for interacting with the GitHub API

[Hub]: http://hub.github.com/

AWS tools:

* [s3cmd] is a command line tool and client for uploading, retrieving and managing data in Amazon S3.

[s3cmd]: https://github.com/s3tools/s3cmd

Programming languages, package managers, and configuration:

* [Asdf] for Ruby version manager
* [Ruby] stable for writing general-purpose code

[Asdf]: https://github.com/asdf-vm/asdf
[Ruby]: https://www.ruby-lang.org/en/

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.
