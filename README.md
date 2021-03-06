GIXY
====
[![Mozilla Public License 2.0](https://img.shields.io/github/license/yandex/gixy.svg?style=flat-square)](https://github.com/yandex/gixy/blob/master/LICENSE)
[![Build Status](https://img.shields.io/travis/yandex/gixy.svg?style=flat-square)](https://travis-ci.org/yandex/gixy)
[![Your feedback is greatly appreciated](https://img.shields.io/maintenance/yes/2017.svg?style=flat-square)](https://github.com/yandex/gixy/issues/new)
[![GitHub issues](https://img.shields.io/github/issues/yandex/gixy.svg?style=flat-square)](https://github.com/yandex/gixy/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/yandex/gixy.svg?style=flat-square)](https://github.com/yandex/gixy/pulls)

# Overview
<img align="right" width="192" height="192" src="/docs/logo.png">

Gixy is a tool to analyze Nginx configuration.
The main goal of Gixy is to prevent misconfiguration and automate flaw detection.

Currently supported Python versions are 2.7 and 3.5+.

Disclaimer: Gixy is well tested only on GNU/Linux, other OSs may have some issues.

# Installation
Gixy is distributed on [PyPI](https://pypi.python.org/pypi/gixy). The best way to install it is with pip:
```bash
pip install gixy
```

Run Gixy and check results:
```bash
gixy
```

# Usage
By default Gixy will try to analyze Nginx configuration placed in `/etc/nginx/nginx.conf`.

But you can always specify needed path:
```
$ gixy /etc/nginx/nginx.conf

==================== Results ===================

Problem: [http_splitting] Possible HTTP-Splitting vulnerability.
Description: Using variables that can contain "\n" may lead to http injection.
Additional info: https://github.com/yandex/gixy/blob/master/docs/ru/plugins/httpsplitting.md
Reason: At least variable "$action" can contain "\n"
Pseudo config:
include /etc/nginx/sites/default.conf;

	server {

		location ~ /v1/((?<action>[^.]*)\.json)?$ {
			add_header X-Action $action;
		}
	}


==================== Summary ===================
Total issues:
    Unspecified: 0
    Low: 0
    Medium: 0
    High: 1
```

Or skip some tests:
```
$ gixy --skips http_splitting /etc/nginx/nginx.conf

==================== Results ===================
No issues found.

==================== Summary ===================
Total issues:
    Unspecified: 0
    Low: 0
    Medium: 0
    High: 0
```

Or something else, you can find all other `gixy` arguments with the help command: `gixy --help`

# Documentation
Full documentation and recommendations can be found [here](https://github.com/yandex/gixy/blob/master/docs/ru/README.md) (sorry, but Russian language only so far)

# Contributing
Contributions to Gixy are always welcome! You can help us in different ways:
  * Open an issue with suggestions for improvements and errors you're facing;
  * Fork this repository and submit a pull request;
  * Improve the documentation.

Code guidelines:
  * Python code style should follow [pep8](https://www.python.org/dev/peps/pep-0008/) standards whenever possible;
  * Pull requests with new plugins must have unit tests for it.
