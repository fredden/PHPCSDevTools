PHPCSDevTools for developers of PHP_CodeSniffer sniffs
=====================================================

[![Latest Stable Version](https://poser.pugx.org/phpcsstandards/phpcsdevtools/v/stable)](https://packagist.org/packages/phpcsstandards/phpcsdevtools)
[![Travis Build Status](https://travis-ci.com/PHPCSStandards/PHPCSDevTools.svg?branch=master)](https://travis-ci.com/PHPCSStandards/PHPCSDevTools/branches)
[![Release Date of the Latest Version](https://img.shields.io/github/release-date/PHPCSStandards/PHPCSDevTools.svg?maxAge=1800)](https://github.com/PHPCSStandards/PHPCSDevTools/releases)
:construction:
[![Latest Unstable Version](https://img.shields.io/badge/unstable-dev--develop-e68718.svg?maxAge=2419200)](https://packagist.org/packages/phpcsstandards/phpcsdevtools#dev-develop)
[![Travis Build Status](https://travis-ci.com/PHPCSStandards/PHPCSDevTools.svg?branch=develop)](https://travis-ci.com/PHPCSStandards/PHPCSDevTools/branches)
[![Last Commit to Unstable](https://img.shields.io/github/last-commit/PHPCSStandards/PHPCSDevTools/develop.svg)](https://github.com/PHPCSStandards/PHPCSDevTools/commits/develop)

[![Minimum PHP Version](https://img.shields.io/packagist/php-v/phpcsstandards/phpcsdevtools.svg?maxAge=3600)](https://packagist.org/packages/phpcsstandards/phpcsdevtools)
[![Tested on PHP 5.4 to 7.4 snapshot](https://img.shields.io/badge/tested%20on-PHP%205.4%20|%205.5%20|%205.6%20|%207.0%20|%207.1%20|%207.2%20|%207.3%20|%207.4snapshot-brightgreen.svg?maxAge=2419200)](https://travis-ci.com/PHPCSStandards/PHPCSDevTools)

[![License: LGPLv3](https://poser.pugx.org/phpcsstandards/phpcsdevtools/license.png)](https://github.com/PHPCSStandards/PHPCSDevTools/blob/master/LICENSE)
![Awesome](https://img.shields.io/badge/awesome%3F-yes!-brightgreen.svg)


This is a set of tools to aid developers of sniffs for [PHP CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer).

* [Installation](#installation)
    + [Composer Project-based Installation](#composer-project-based-installation)
    + [Composer Global Installation](#composer-global-installation)
    + [Stand-alone Installation](#stand-alone-installation)
* [Features](#features)
    + [Checking whether all sniffs in a PHPCS standard are feature complete](#checking-whether-all-sniffs-in-a-phpcs-standard-are-feature-complete)
    + [PHPCSDev ruleset for sniff repos](#phpcsdev-ruleset-for-sniff-repos)
* [Contributing](#contributing)
* [License](#license)


Installation
-------------------------------------------

### Composer Project-based Installation

Run the following from the root of your project:
```bash
composer require phpcsstandards/phpcsdevtools:^1.0
```

### Composer Global Installation

If you work on several different sniff repos, you may want to install this toolset globally:
```bash
composer global require phpcsstandards/phpcsdevtools:^1.0
```

### Stand-alone Installation

* Install [PHP CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) via [your preferred method](https://github.com/squizlabs/PHP_CodeSniffer#installation).
* Register the path to PHPCS in your system `$PATH` environment variable to make the `phpcs` command available from anywhere in your file system.
* Download the [latest PHPCSDevTools release](https://github.com/PHPCSStandards/PHPCSDevTools/releases) and unzip/untar it into an arbitrary directory.
    You can also choose to clone this repository using git.
* Add the path to the directory in which you placed your copy of the PHPCSDevTools repo to the PHP CodeSniffer configuration using the below command:
   ```bash
   phpcs --config-set installed_paths /path/to/PHPCSDevTools
   ```
   **Warning**: :warning: The `installed_paths` command overwrites any previously set `installed_paths`. If you have previously set `installed_paths` for other external standards, run `phpcs --config-show` first and then run the `installed_paths` command with all the paths you need separated by comma's, i.e.:
   ```bash
   phpcs --config-set installed_paths /path/1,/path/2,/path/3
   ```


Features
------------------------------

### Checking whether all sniffs in a PHPCS standard are feature complete

You can now easily check whether each and every sniff in your standard is accompanied by a documentation XML file (warning) as well as unit test files (error).

To use the tool, run it from the root of the your standards repo like so:
```bash
# When installed as a project dependency:
vendor/bin/phpcs-check-feature-completeness

# When installed globally with Composer:
phpcs-check-feature-completeness

# When installed as a git clone or otherwise:
php -f "path/to/PHPCSDevTools/bin/phpcs-check-feature-completeness"
```

If all is good, you will see a `All # sniffs are accompanied by unit tests and documentation.` message.

If there are files missing, you will see errors/warnings for each missing file, like so:
```
WARNING: Documentation missing for path/to/project/StandardName/Sniffs/Category/SniffNameSniff.php.
ERROR: Unit tests missing for path/to/project/StandardName/Sniffs/Category/SniffNameSniff.php.
```

For the fastest results, it is recommended to pass the name of the subdirectory where your standard is located to the script, like so:
```bash
phpcs-check-feature-completeness ./StandardName
```

#### Options
```
directories   One or more specific directories to examine.
              Defaults to the directory from which the script is run.
-q, --quiet   Turn off warnings for missing documentation.
--exclude     Comma-delimited list of (relative) directories to exclude
              from the scan.
              Defaults to excluding the /vendor/ directory.
--no-progress Disable progress in console output.
--colors      Enable colors in console output.
              (disables auto detection of color support)
--no-colors   Disable colors in console output.
-v            Verbose mode.
-h, --help    Print this help.
-V, --version Display the current version of this script.
```


### PHPCSDev ruleset for sniff repos

Once this project is installed, you will see a new `PHPCSDev` ruleset in the list of installed standards when you run `phpcs -i`.

**Important: This ruleset currently requires PHP_CodeSniffer >= `3.4.0+`.**

> As sniffs developers will mostly work with the latest version of PHP_CodeSniffer, this shouldn't cause any problems.
>
> Similarly, the CS check in automated CI runs should normally be run on a high PHPCS version for the best results.

The `PHPCSDev` standard can be used by sniff developers to check the code style of their sniff repo code.

Often, sniff repos will use the code style of the standard they are adding. However, not all sniff repos are actually about code style.

So for those repos which need a basic standard which will still keep their code-base consistent, this standard should be useful.

The standard checks your code against the following:
* Compliance with [PSR-2](https://www.php-fig.org/psr/psr-2/).
* Use of camelCase variable and function names.
* Use of normalized arrays.
* All files, classes, functions and properties are documented with a docblock and contain the minimally needed information.
* A small number of arbitrary additional code style checks.
* PHP cross-version compatibility, while allowing for the tokens back-filled by PHPCS itself.
    Note: for optimal results, the project custom ruleset should set the `testVersion` config variable.
    This is not done by default as config variables are currently [difficult](https://github.com/squizlabs/PHP_CodeSniffer/issues/2197) [to overrule](https://github.com/squizlabs/PHP_CodeSniffer/issues/1821).

The ruleset can be used like any other ruleset and specific sniffs and settings can be added to or overruled from a custom project based ruleset.

For an example project-based ruleset using the `PHCPSDev` standard, have a look at the [`phpcs.xml.dist` file](https://github.com/PHPCSStandards/PHPCSDevTools/blob/develop/phpcs.xml.dist) in this repo.


Contributing
-------
Contributions to this project are welcome. Just clone the repo, branch off from `develop`, make your changes, commit them and send in a pull request.

If unsure whether the changes you are proposing would be welcome, open an issue first to discuss your proposal.

License
-------
This code is released under the GNU Lesser General Public License (LGPLv3). For more information, visit http://www.gnu.org/copyleft/lesser.html
