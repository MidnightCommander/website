# Translations

We use [Transifex](https://www.transifex.com) to crowdsource translations.

Transifex allows us to centralize translation activities and enables translators without much previous experience to edit our localization files via the web interface. Translators who want to work directly with PO files can download the latest versions from Transifex, make their changes using familiar tools, and upload them back.

To join our project, go to our [project page](https://explore.transifex.com/mc/mc/) and click on the "Join this project" button. Join requests for new contributors are approved by the maintainers as time permits.


## Transifex client

As of 2025, Fedora 41 unfortunately only ships the old client (`transifex-client`), which is no longer useful.  The new client is available in [binary form from GitHub](https://github.com/transifex/cli/releases) (download, unpack and copy to `~/bin`). To complete the configuration, fill in your API token:

```shell title="~/.transifexrc"
[https://www.transifex.com]
rest_hostname = https://rest.api.transifex.com
token         = 1/***
```

To make sure that Transifex client is installed correctly, try to run it with the following command:

```shell
tx --help
```
