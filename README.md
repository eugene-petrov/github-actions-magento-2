# GitHub Actions for Magento 2 Projects

This repository's aim is to provide a set of open sourced GitHub actions to write better tested Magento 2 Project.

# Available Actions

## Magento Coding Standard
Provides an action that can be used in your GitHub workflow to execute the latest [Magento Coding Standard](https://github.com/magento/magento-coding-standard).

### How to use it
In your GitHub repository add the below as
`.github/workflows/coding-standard.yml`

```yaml
name: M2 Coding Standard
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  coding-standard-code:
    name: M2 Coding Standard - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/coding-standard@main
        with:
          path_to_code: /app/code
          phpcs_extensions: php,phtml,graphqls/GraphQL,less/CSS,xml,js/PHP
  coding-standard-design:
    name: M2 Coding Standard - Design
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/coding-standard@main
        with:
          path_to_code: /app/design
          phpcs_extensions: php,phtml,graphqls/GraphQL,less/CSS,xml,js/PHP
```
---

## Magento Mess Detector
Provides an action that can be used in your GitHub workflow to execute the [PHP Mess Detector rules](https://github.com/magento/magento2/blob/2.4.6/dev/tests/static/testsuite/Magento/Test/Php/_files/phpmd/ruleset.xml) included in Magento 2.

### How to use it
In your GitHub repository add the below as
`.github/workflows/mess-detector.yml`

```yaml
name: M2 Mess Detector
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  mess-detector-code:
    name: M2 Mess Detector - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/mess-detector@main
        with:
          path_to_code: /app/code
  mess-detector-design:
    name: M2 Mess Detector - Design
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/mess-detector@main
        with:
          path_to_code: /app/design
```

In the example above, the specific rules provided by Magento will not be used for checking.

- [AllPurposeAction](https://github.com/magento/magento2/blob/2.4.6/dev/tests/static/framework/Magento/CodeMessDetector/Rule/Design/AllPurposeAction.php) - Actions must process a defined list of HTTP methods.
- [CookieAndSessionMisuse](https://github.com/magento/magento2/blob/2.4.6/dev/tests/static/framework/Magento/CodeMessDetector/Rule/Design/CookieAndSessionMisuse.php) - Session and Cookies must be used only in HTML Presentation layer.

In order for these rules to also be included in the check, use the following action.

In your GitHub repository add the below as
`.github/workflows/mess-detector.yml`

```yaml
name: M2 Mess Detector
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  mess-detector-full-code:
    name: M2 Mess Detector Full - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/mess-detector-full@main
        with:
          path_to_code: /app/code
  mess-detector-full-design:
    name: M2 Mess Detector - Design
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/mess-detector-full@main
        with:
          path_to_code: /app/design
```

During the operation of this action, the `composer install` command is executed, which leads to an increase in execution time. It is also possible to use a mixed workflow: check the `app/code` folder with a full action, and the `app/design` folder with a stripped-down one, since specific rules do not affect files in the `app/design` folder.

---

## PHP Compatibility
Provides an action that can be used in your GitHub workflow to execute the [PHP Compatibility rules](https://github.com/PHPCompatibility/PHPCompatibility) for specific PHP version.

### How to use it
In your GitHub repository add the below as
`.github/workflows/php-compatibility.yml`

```yaml
name: PHP Compatibility
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  php-compatibility-code:
    name: PHP Compatibility - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/php-compatibility@main
        with:
          path_to_code: /app/code
          php_versions: 7.4-
  php-compatibility-design:
    name: PHP Compatibility - Design
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/php-compatibility@main
        with:
          path_to_code: /app/design
          php_versions: 7.4-
```

### Sniffing your code for compatibility with specific PHP version(s)

- To get the most out of the PHPCompatibility standard, you should [specify](https://github.com/PHPCompatibility/PHPCompatibility#sniffing-your-code-for-compatibility-with-specific-php-versions) a `php_versions` to check against. That will enable the checks for both deprecated/removed PHP features as well as the detection of code using new PHP features.
    - You can run the checks for just one specific PHP version by adding `php_versions: 7.4` to step arguments.
    - You can also specify a range of PHP versions that your code needs to support. In this situation, compatibility issues that affect any of the PHP versions in that range will be reported: `php_versions: 7.4-8.2`.
    - You can omit one part of the range if you want to support everything above or below a particular version, i.e. use `php_versions: 7.4-` to run all the checks for PHP 7.4 and above.
---

## Inline Styles
Provides an action that can be used in your GitHub workflow to detect inline styles.

### How to use it
In your GitHub repository add the below as
`.github/workflows/inline-styles.yml`

```yaml
name: M2 Inline Styles
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  coding-standard-code:
    name: M2 Inline Styles - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/coding-standard@main
        with:
          path_to_code: /app/code
          phpcs_extensions: php/InlineCss,phtml/InlineCss,html/InlineCss
  coding-standard-design:
    name: M2 Inline Styles - Design
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/coding-standard@main
        with:
          path_to_code: /app/design
          phpcs_extensions: php/InlineCss,phtml/InlineCss,html/InlineCss
```
---

## Unnecessary Files
Provides an action that can be used in your GitHub workflow to detect unnecessary files.

### How to use it
In your GitHub repository add the below as
`.github/workflows/unnecessary-files.yml`

```yaml
name: Unnecessary Files
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  unnecessary-files:
    name: Unnecessary Files
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/coding-standard@main
        with:
          path_to_code: /
          phpcs_extensions: gz/FileType,tar/FileType,rar/FileType,zip/FileType,exe/FileType,tgz/FileType,tlz/FileType,tbz2/FileType,bak/FileType,back/FileType,asp/FileType,pass/FileType,shar/FileType,iso/FileType,bz2/FileType,lz/FileType,lz4/FileType,lzma/FileType,lzo/FileType,sz/FileType,xz/FileType,z/FileType,zst/FileType,7z/FileType,s7z/FileType,jar/FileType,sql/FileType
```
---

## Magento PHP Stan
Provides an action that can be used in your GitHub workflow to execute the [PHPStan rules](https://github.com/magento/magento2/blob/2.4.6/dev/tests/static/framework/Magento/TestFramework/CodingStandard/Tool/PhpStan.php) included in Magento 2.

### How to use it
In your GitHub repository add the below as
`.github/workflows/phpstan.yml`

```yaml
name: M2 PHPStan
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  phpstan-code:
    name: M2 PHPStan - Code
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: 121eCommerceLLC/github-actions-magento-2/phpstan@main
        with:
          path_to_code: /app/code
          level: 1
```

If you want to use PHPStan but your codebase isn’t up to speed with strong typing and PHPStan’s strict checks, you can currently choose from 10 [levels](https://phpstan.org/user-guide/rule-levels) (`0` is the loosest and `9` is the strictest) by passing `level` to the action.

The default level is `1`. You can also use `level: max` as an alias for the highest level. This will ensure that you will always use the highest level when upgrading to new versions of PHPStan.

> Please note that this can create a significant obstacle when upgrading to a newer version because you might have to fix a lot of code to bring the number of errors down to zero.

---

## Customization

The provided actions have the `path_to_code` argument to run the check on a specific folder. In the examples, all actions are run for the `app/code` and `app/design` folders, but in some situations we may need to run a check on some subfolders, for example, third-party modules were installed in the `app/code` folder, and now when running checks there are errors unrelated to our code because of which we cannot make a merge of pull request, as this may be restricted in the repository settings. For such cases, we can simply specify the folder `app/code/Ecommerce121` and thereby circumvent this restriction. 

The same situation is with the `app/desing` folder, if we have several themes installed, but we only want to check a certain one, then we can simply specify the path to our theme.

If we need to check several folders, then we can add them all within one workflow, but as separate steps.

---

## How to run locally

### PHP Code Sniffer
```shell
./vendor/bin/phpcs -p --colors --extensions=php,phtml,graphqls/GraphQL,less/CSS,xml,js/PHP --standard=Magento2 --exclude=Magento2.Annotation.MethodAnnotationStructure app/code/Ecommerce121/Module/
```

### PHP Mess Detector
```shell
./vendor/bin/phpmd app/code/Ecommerce121/Module/ ansi dev/tests/static/testsuite/Magento/Test/Php/_files/phpmd/ruleset.xml --suffixes php,phtml
```

### PHP Compatibility
```shell
./vendor/bin/phpcs -p --colors --extensions=php,phtml --standard=PHPCompatibility --runtime-set testVersion 7.4- app/code/Ecommerce121/Module/
```

### PHP Stan
To run `phpstan`, the preparation commands must be executed:
```shell
composer require --dev phpstan/phpstan --no-install
composer require --dev bitexpert/phpstan-magento --no-install
composer require --dev phpstan/extension-installer --no-install
composer config --no-plugins allow-plugins.phpstan/extension-installer true
composer install
```

Then the following command can be executed:
```shell
./vendor/bin/phpstan analyze app/code/Ecommerce121/Module/ --level 1 --configuration dev/tests/static/testsuite/Magento/Test/Php/_files/phpstan/phpstan.neon
```
