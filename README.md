# Hugo Snap Test

A site to test the Hugo [snap] package.

> A snap is a bundle of an app and its dependencies that works without
modification across Linux distributions.

## Instructions

1\) Install the extended version of Hugo from the [Snap Store].

```bash
sudo snap install hugo --channel=extended/stable
```

2\) Clone this repository, install the npm packages, and build the site.

```bash
git clone https://github.com/jmooring/hugo-snap-test
cd hugo-snap-test
npm install
/snap/bin/hugo
```

To test without triggering the Go error:

```bash
HUGO_MODULE_IMPORTS="" /snap/bin/hugo
```

To test without triggering the Go and Embedded Dart Sass errors:

```bash
HUGO_MODULE_IMPORTS="" HUGO_PARAMS_TRANSPILER="libsass" /snap/bin/hugo
```

## Background

Some Hugo features have one or more external dependencies. For example, to
extract a page's last commit date from your project's git repository, Hugo must
be able to call Git.

The Hugo snap package is subject to [strict confinement]. Under strict
confinement, the snap must include the dependencies below, excluding the npm
packages (PostCSS and PostCSS CLI).

To clarify, even if you have manually installed these dependencies, building
the site with the Hugo snap will throw errors unless the snap has bundled these
dependencies.

Dependency|Description
:--|:--
[Asciidoctor]|Required to render [AsciiDoc] markdown. Requires Ruby.
[Embedded Dart Sass]|Required to transpile [Sass] to CSS using [Dart Sass] instead of [LibSass].
[Git]|Required to access [Git Info Variables] and download [Hugo Modules].
[Go]|Required to create and manage [Hugo Modules].
[Node.js]|Required by PostCSS and PostCSS CLI.
[Pandoc]|Required to render Pandoc markdown.
[PostCSS]|Required to process CSS files with PostCSS using [Hugo Pipes]. This is an [npm] package. Requires Node.js.
[PostCSS CLI]|Required to process CSS files with PostCSS using [Hugo Pipes].This is an [npm] package. Requires Node.js.
[Python]|Required by rst2html.
[rst2html]|Required to render [reStructuredText] markdown. Requires Python.
[Ruby]|Required by Asciidoctor.

[AsciiDoc]: https://docs.asciidoctor.org/asciidoc/latest/
[Asciidoctor]: https://asciidoctor.org/
[classic confinement]: https://snapcraft.io/docs/snap-confinement
[Dart Sass]: https://sass-lang.com/dart-sass
[Embedded Dart Sass]: https://github.com/sass/dart-sass-embedded/
[Git Info Variables]: https://gohugo.io/variables/git/
[Git]: https://git-scm.com/
[Go]: https://golang.org/
[Hugo Modules]: https://gohugo.io/hugo-modules/
[Hugo Pipes]: https://gohugo.io/hugo-pipes/postcss
[LibSass]: https://sass-lang.com/libsass
[Node.js]:https://nodejs.org/
[npm]: https://www.npmjs.com/
[Pandoc]: https://pandoc.org/
[PostCSS CLI]: https://www.npmjs.com/package/postcss-cli
[PostCSS]: https://www.npmjs.com/package/postcss
[Python]: https://www.python.org/
[reStructuredText]: https://docutils.sourceforge.io/rst.html
[rst2html]: https://pypi.org/project/rst2html/
[Ruby]: https://www.ruby-lang.org/
[Sass]: https://sass-lang.com/
[Snap Store]: https://snapcraft.io/hugo
[snap]: https://snapcraft.io/about
[strict confinement]: https://snapcraft.io/docs/snap-confinement

This site will not build unless all dependencies are available.

This site:

- Includes an AsciiDoc markdown page
- Includes a Pandoc markdown page
- Includes a reStructuredText markdown page
- Pulls content from a Hugo Module
- Accesses Git Info Variables
- Uses Embedded Dart Sass to transpile Sass to CSS
- Uses the PostCSS autoprefixer plugin to add vendor prefixes to the CSS

## Installing Dependencies Manually

If you are not using the Hugo snap package, you must manually install the
dependencies if you wish to build this site. These instructions are for Ubuntu, tested with Ubuntu 20.04.3 LTS.

### Asciidoctor

:information_source: This will also install Ruby if Ruby is not already installed.

```bash
sudo apt install asciidoctor
asciidoctor --version
```

### Embedded Dart Sass

```bash
base=https://github.com && temp=dart-sass-embedded.tar.gz
path=$(curl -sL https://github.com/sass/dart-sass-embedded/releases/latest | grep "href.*linux-x64.tar.gz" | awk -F'"' '{print $2}')
wget -qO $temp $base$path
tar -xvzf $temp
sudo mv sass_embedded/dart-sass-embedded /usr/local/bin/
sudo chmod 755 /usr/local/bin/dart-sass-embedded
rm $temp
rm -r sass_embedded/ 
dart-sass-embedded --version
```

### Git

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt install git
git --version
git config --global core.quotepath off
git config --global init.defaultBranch "main"
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

### Go

Add this section to $HOME/.bashrc:

```text
# Set PATH to include $HOME/go/bin if it exists.
if [ -d "$HOME/go/bin" ]; then
  PATH="$HOME/go/bin:$PATH"
fi
```

Install and verify:

```bash
sudo snap install go --channel=1.18/stable --classic
go version
```

### Node.js

Add this section to $HOME/.bashrc:

```text
# Set PATH to include $HOME/.npm-global/bin if it exists.
if [ -d "$HOME/.npm-global/bin" ]; then
  PATH="$HOME/.npm-global/bin:$PATH"
fi
```

Install and verify:

```bash
sudo snap install node --channel=16/stable --classic
mkdir -p $HOME/.npm-global
npm config set prefix $HOME/.npm-global
node --version
```

### Pandoc

```bash
sudo apt install pandoc
pandoc --version
```

### rst2html

:information_source: This will also install Python if Python is not already installed.

Add this section to $HOME/.bashrc:

```text
# Set PATH to include $HOME/.local/bin if it exists.
if [ -d "$HOME/.local/bin" ];then
  PATH="$HOME/.local/bin:$PATH"
fi
```

Install and verify:

```bash
sudo apt install python3-pip
pip install docutils
source "$HOME/.bashrc"
rst2html.py --version
```
