# Thoughts & Ideas WordPress Coding Standards

[![license](https://img.shields.io/github/license/thoughtsideas/ti-wpcs.svg)](https://github.com/thoughtsideas/ti-wpcs)  [![GitHub release](https://img.shields.io/github/release/thoughtsideas/ti-wpcs.svg)](https://github.com/thoughtsideas/ti-wpcs)  [![Packagist](https://img.shields.io/packagist/v/thoughtsideas/ti-wpcs.svg)](https://packagist.org/packages/thoughtsideas/ti-wpcs)  [![Packagist](https://img.shields.io/packagist/dt/thoughtsideas/ti-wpcs.svg)](https://packagist.org/packages/thoughtsideas/ti-wpcs)  [![GitHub issues](https://img.shields.io/github/issues/thoughtsideas/ti-wpcs.svg)](https://github.com/thoughtsideas/ti-wpcs)  [![Libraries.io for GitHub](https://img.shields.io/librariesio/github/thoughtsideas/ti-wpcs.svg)](https://github.com/thoughtsideas/ti-wpcs)

Thoughts & Ideas WordPress Coding Standards (TI-WPCS) is a development
dependency for testing coding styles and quality in our WordPress PHP projects.
Keeping the code base consistent and highlighting potential issues before
they enter our products helps developers pickup our projects and improve
their reliability in production.

## Installation

*Thoughts & Ideas recommend installing this dependancy via Composer.*

### As a Composer Dependancy

To include these standards as part of a project. Require this repository
as a development dependancy:

```Shell
composer require --dev thoughtsideas/ti-wpcs
```

The fixer and tests can be run from the command line:

```Shell
./vendor/bin/phpcbf ./
./vendor/bin/phpcs ./
./vendor/bin/phpmd ./ text ./vendor/thoughtsideas/ti-wpcs/ti-wpmd/ruleset.xml
./vendor/bin/phpcpd ./ --regexps-exclude=#vendor/#,#node_modules/# --progress
./vendor/bin/security-checker security:check composer.lock
./vendor/bin/phpmnd ./ --ignore-funcs=round,sleep --exclude=./vendor/ --progress
```

Or added to the Composer scripts section:

```JSON
scripts: {
  "test-phpcbf": "./vendor/bin/phpcbf ./ --standard=vendor/thoughtsideas/TI-WPCS/ti-wpcs/ruleset.xml",
  "test-phpcs": "./vendor/bin/phpcs ./ --standard=vendor/thoughtsideas/TI-WPCS/ti-wpcs/ruleset.xml",
  "test-phpmd": "./vendor/bin/phpmd ./ text ./vendor/thoughtsideas/ti-wpcs/TI-WPMD/ruleset.xml",
  "test-phpcpd": "./vendor/bin/phpcpd ./ --regexps-exclude=#vendor/#,#node_modules/# --progress",
  "test-phpsc": "./vendor/bin/security-checker security:check composer.lock",
  "test-phpmnd": "./vendor/bin/phpmnd ./ --ignore-funcs=round,sleep --exclude=./vendor/ --progress",

  "test": [
    "composer run test-phpcbf",
    "composer run test-phpcs",
    "composer run test-phpmd",
    "composer run test-phpcpd",
    "composer run test-phpsc",
    "composer run test-phpmnd"
  ]
}
```

### Automatically run tests before every Git commit.

*Thoughts & Ideas recommend a pre-commit Git Hook.*

Create the pre-commit hook file:

```Shell
mkdir ./_scripts && touch ./_scripts/pre-commit
```

Add the pre-commit hook to our new file:

```Shell
#!/bin/sh
#
# Pre Commit Hook.

# Remove un-staged changes.
git stash -q --keep-index

# Run QA script.
echo "Running WordPress PHP Coding Standards test"
composer run test-phpcbf
composer run test-phpcs
# PHPCS Results (bool)
QA_PHPCS=$?

echo "Running WordPress PHP Mess Detector test"
composer run test-phpmd
# PHPMD Results (bool)
QA_PHPMD=$?

echo "Running WordPress PHP Copy Paste Detector test"
composer run test-phpcpd
# PHPCPD Results (bool)
QA_PHPCPD=$?

echo "Running WordPress PHP Security Checker test"
composer run test-phpsc
# PHPSC Results (bool)
QA_PHPSC=$?

echo "Running WordPress PHP Magic Number Detector test"
composer run test-phpmnd
# PHPMND Results (bool)
QA_PHPMND=$?

# Apply un-staged changes.
git stash pop -q

# Get our test QA results.
[ $QA_PHPCS -ne 0 ] && exit 1
[ $QA_PHPMD -ne 0 ] && exit 1
[ $QA_PHPCPD -ne 0 ] && exit 1
[ $QA_PHPSC -ne 0 ] && exit 1
[ $QA_PHPMND -ne 0 ] && exit 1

# If test pass exit successfully.
exit 0
```

Link our new file to our git repository

```Shell
ln -s ../../_scripts/pre-commit ./.git/hooks/pre-commit
```

Every time you commit to your Git repository the TI-WPCS test will
run automatically.

## Reporting Issues

If you spot any issues please create a ticket via [GitHub's Issue Tracker](https://github.com/thoughtsideas/ti-wpcs/issues).
Including the issue, the browser and operating system, and how to replicate it.
If the issue is security related please use the contact information below.

## Contact

Thoughts & Ideas - [hello@thoughtsideas.uk](hello@thoughtsideas.uk)

## License

This project is licensed under the terms of the MIT license. See the [LICENSE](https://github.com/thoughtsideas/ti-wpcs/blob/master/license.txt) file.

## Copyright

Unless otherwise stated © Thoughts & Ideas. All rights reserved.
