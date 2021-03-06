## PHP General


### PHP CLI vs PHP CGI

- PHP can be installed via:

```bash
$ sudo zypper install php7
```

- this command installs the CLI (Command Line Interface) version, which doesn't process GET/POST data -- it's sort of a clean PHP interpreter, completely unaware of HTTP
- this command is identical to:

```bash
$ sudo zypper install php7-cli
```

- for web development purposes, one needs to install the CGI version:

```bash
$ sudo zypper install php7-cgi
```

- if the php7-cgi package is not available, install the php7 version and then install the suggested packages:

```
The following 10 NEW packages are going to be installed:
  php7 php7-ctype php7-dom php7-iconv php7-json php7-pdo php7-sqlite php7-tokenizer php7-xmlreader php7-xmlwriter

The following 8 recommended packages were automatically selected:
  php7-ctype php7-dom php7-iconv php7-json php7-sqlite php7-tokenizer php7-xmlreader php7-xmlwriter

The following 11 packages are suggested, but will not be installed:
  php5-gd php5-gettext php5-mbstring php5-mysql php5-pear php5-suhosin php7-gd php7-gettext php7-mbstring php7-mysql php7-pear

10 new packages to install.
Overall download size: 2.0 MiB. Already cached: 0 B. After the operation, additional 5.3 MiB will be used.
```

### Visual Studio Code

- go to File... Preference... Workspace Settings... and add this line in the settings.json file: ```"php.validate.executablePath": "/usr/bin/php"
	- *Note:* be mindful about the .json files rules -- that entries must be separated by a ```,```, but if it's the last line, it shouldn't have any punctuation
- for Windows systems the setting should be ```"php.validate.executablePath": "c:\\xampp\\php\\php.exe"
- more info on [VS Code webpage](https://code.visualstudio.com/Docs/languages/php) and on [Nick Trogh's blog](https://blogs.msdn.microsoft.com/nicktrog/2016/02/11/configuring-visual-studio-code-for-php-development/)






