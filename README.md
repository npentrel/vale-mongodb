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
   git clone git@github.com:10gen/mongodb-vale.git
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
4. To ensure these symlinks do not accidentally get added to git run:
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
   vale-check
   whitespace-check" >> $GITIGNOREFILE
   ```

   This script checks if you already have a global git ignore file and
   sets one up if you dont. It then adds the files we created symlinks
   for to your global gitignore file.
5. Install `rst2html`. This allows vale to interpret `rst` files.

   You can check if you already have `rst2html` installed
   by running `which rst2html` into your terminal. If you don't have
   rst2html installed run `pip3 install rst2html` or `pip install rst2html`.

   Try running `which rst2html` again. If you do not get a path to the
   command as the output, try running `which rst2thml.py`. If the
   second command worked, create a symlink for `rst2html.py`:
   ```sh
   sudo ln -s rst2html.py /usr/local/bin/rst2html
   # you need to enter your password to confirm this
   ```

   Again try running `which rst2html`. If it doesn't work (or if `which
   rst2html.py` also didn't work for you), you need to add the folder
   that contains `rst2html` to your PATH. Open up either your `~/.bashrc`
   or `~/.zshrc` file (depending on what you use) and add the following
   lines at the bottom of the file, **swapping out the `/usr/local/bin/`
   with the path to your `rst2html` file**:
   ```sh
   # Append path to rst2html for vale
   export PATH=$PATH:/usr/local/bin/
   ```

   Do not proceed until you can run `which rst2html`. Installing this
   ensures things like codeblocks aren't checked against the style guide
   (- unfortunately this doesn't work for all codeblocks - any
   Sphinx-modifiers like `:copyable:` mean the codeblocks are not
   ignored - if this bothers you we'll need to see if we can get budget
   to pay for `vale-server` which would allow us to integrate with Sphinx).

6. To run `vale` against a file use:
   ```sh
   vale /path/to/file
   ```
   Inside the mongodb-vale repo, you can run `vale test.rst` to run vale on an
   example file. The output should end with the following line:

   ```sh
   ✖ 1 error, 1 warning and 1 suggestion in 1 file.
   ```

7. If you want to run vale against all files that have changed since a given commit hash use the `vale-check` wrapper:
   ```sh
   ./vale-check <HASH>
   # example: ./vale-check
   # example: ./vale-check HEAD
   # example: ./vale-check HEAD~3
   # example: ./vale-check abc123
   ```

   This will also run a whitespace check on the files.

   Inside the mongodb-vale repo, you can run `./vale-check 36b93db2` to run vale on an
   example file. You should see the same output as in step 6 with an
   additiona note that `You missed a new line after a directive.`.

## Usage

To learn how to use Vale, see [Usage](https://github.com/errata-ai/vale/#usage).

### Turning Rules Off

To turn off a given rule open the `.vale.ini` file and set the respective variable to `NO`. Example:

```ini
MongoDB.Abbreviations = NO
```

### Using Vale in VSCode

> **NOTE**: Please follow the steps in Getting Started first!

1. Install the [vale extension](https://marketplace.visualstudio.com/items?itemName=errata-ai.vale-server)
2. Navigate to the vale extension settings:
   * Turn on `Use CLI`
   * Set `Lint Context` to a reasonable number like `30`
   * Specify the absolute path to the `.vale.ini` file for `Vale CLI: Config`. The file is in the repo you cloned in `Getting Started`.
   * Set the min alert level to `suggestion` - a lot of the rules are coded as suggestions.
   * Specify the path to your vale installation. To find this path run `which vale` in your terminal and copy the path.
     This path can't include any spaces before or after the path.


### Using Vale-Server

After the 30 day trial period, a license for Vale server can be obtained
[here](https://errata.ai/vale-server/#purchase). Vale server allows you
to also make use of [LanguageTool's open-source grammar
checker](https://docs.errata.ai/vale-server/add-ons/languagetool).

To install Vale Server, follow [these instructions](https://docs.errata.ai/vale-server/install)
and use the `.vale.ini` file from the repo you cloned. Swap out the path to the
styles (`StylesPath = vale/styles`) in line 4 with the absolute path to
the file (in my case `StylesPath = /Users/naomi/coding/vale-mongodb/vale/styles`).

You can [install LanguageTool for macOS](https://languagetool.org/download/LanguageTool-stable.zip).
Follow the [instructions to integrate LanguageTool with Vale-Server](https://docs.errata.ai/vale-server/add-ons/languagetool).

If you set up Vale for VS-Code you can point it to your vale-server
installation to make use of the extra features (via the extension settings).

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
