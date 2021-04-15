> **NOTE**: This project is neither officially maintained nor endorsed by MongoDB.

This repository contains a [Vale-compatible](https://github.com/errata-ai/vale) implementation of the [*MongoDB Documentation Style Guide*](https://docs.mongodb.com/meta/style-guide/).

The goal of this project is to provide a helpful style guide adherance checker.

[![Build Status](https://travis-ci.org/errata-ai/Google.svg?branch=master)](https://travis-ci.org/errata-ai/Google) ![Vale version](https://img.shields.io/badge/vale-%3E%3D%20v1.0.0-blue.svg) ![license](https://img.shields.io/github/license/mashape/apistatus.svg)

## Getting Started

> :exclamation: MongoDB requires Vale >= **1.0.0**. :exclamation:

1. Install `vale`. If you do not have `brew` installed follow [these
   installation instructions](https://docs.errata.ai/vale/install).
   ```sh
   brew install vale
   ```
2. In the folder where you keep code, clone this repo:
   ```sh
   git clone git@github.com:npentrel/vale-mongodb.git
   ```
3. Inside a folder that contains docs, create a symbolic link to the `vale` folder, the `.vale.ini` file and to the `vale-check` script:

   ```sh
   ln -s $(pwd)/vale /path/to/docs/folder/
   ln -s $(pwd)/.vale.ini /path/to/docs/folder/
   ln -s $(pwd)/vale-check /path/to/docs/folder/
   ln -s $(pwd)/whitespace-check /path/to/docs/folder/
   # example: ln -s $(pwd)/vale /Users/naomi/coding/docs/
   # example: ln -s $(pwd)/.vale.ini /Users/naomi/coding/docs/
   # example: ln -s $(pwd)/vale-check /Users/naomi/coding/docs/
   # example: ln -s $(pwd)/whitespace-check /Users/naomi/coding/docs/
   ```
4. To ensure these symlinks do not accidentally get added to git add run:
   ```sh
   #!/bin/sh
   GITIGNOREFILE=$(git config --get core.excludesfile)
   if [ -z "$GITIGNOREFILE" ]
   then
         GITIGNOREFILE=~/.gitignore_global
         git config --global core.excludesfile '~/.gitignore_global'
         echo "Created ~/.gitignore_global"
   else
         echo "Appending to existing global gitignore file"
   fi
   GITIGNOREFILE="${GITIGNOREFILE/#\~/$HOME}"
   echo "
   vale
   .vale.ini
   vale-check" >> $GITIGNOREFILE
   ```

   This script checks if you already have a global git ignore file and
   sets one up if you dont. It then adds the files we created symlinks
   for to your global gitignore file.
5. To run `vale` against a file use:
   ```sh
   vale /path/to/file
   ```
6. If you want to run vale against all files that have changed since a given commit hash use the `vale-check` wrapper:
   ```sh
   ./vale-check <HASH>
   # example: ./vale-check
   # example: ./vale-check HEAD
   # example: ./vale-check HEAD~3
   # example: ./vale-check abc123
   ```

## Usage

To turn off a given rule open the `.vale.ini` file and set the respective variable to `NO`. Example:

```ini
MongoDB.Abbreviations = NO
```

See [Usage](https://github.com/errata-ai/vale/#usage) for more information.

## Repository Structure

<dl>
  <dt><a href="https://github.com/npentrel/vale-mongodb/tree/main/vale/styles/MongoDB"><code>/vale/styles/MongoDB</code></a></dt>
  <dd>The <a href="http://yaml.org/">YAML</a>-based rule implementations that make up our style.</dd>

  <dt><a href="https://github.com/npentrel/vale-mongodb/blob/main/.vale.ini"><code>/.vale.ini</code></a></dt>
  <dd>The vale configuration file.</dd>

  <dt><a href="https://github.com/npentrel/vale-mongodb/blob/main/vale-check"><code>/vale-check</code></a></dt>
  <dd>A wrapper script that allows you to run vale and a whitespace-check agains recently changed files in git.</dd>

  <dt><a href="https://github.com/npentrel/vale-mongodb/blob/main/whitespace-check"><code>/whitespace-check</code></a></dt>
  <dd>A script that checks for whitespace issues.</dd>
</dl>

## Credits

Some rules, especially regexes, are based on other project's rules. Where I didn't leave rules in a folder with the name of the creator, I have pointed out in the individual
files where the rules/regexes stem from.

Most commonly reused rules are from:
- [https://github.com/errata-ai/Google](https://github.com/errata-ai/Google)
- [https://github.com/errata-ai/Microsoft](https://github.com/errata-ai/Microsoft)
- [https://gitlab.com/gitlab-org/gitlab/-/tree/master/doc/.vale/gitlab](https://gitlab.com/gitlab-org/gitlab/-/tree/master/doc/.vale/gitlab)