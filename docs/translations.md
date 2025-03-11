# Translations

We use [Transifex](https://www.transifex.com) to crowdsource translations.

Transifex allows us to centralize translation activities and enables translators without much previous experience to edit our localization files via the web interface. Translators who want to work directly with PO files can download the latest versions from Transifex, make their changes using familiar tools, and upload them back.

## Contributing

To join our project, go to our [project page](https://explore.transifex.com/mc/mc/) and click on the "Join this project" button. Join requests for new contributors are processed by the maintainers as time permits.

!!! note
    At the moment it is only possible to translate the interface and hint files!

    Translation of the manual pages from which the online help accessible via ++f1++ is generated has been disabled to protect our translators. Unfortunately, the resources were manually uploaded to Transifex by a former member of the development team, but the corresponding [sync code]({{ config.repo_url }}/tree/master/maint/sync-transifex) for the manual pages was never developed.

    Will you be the hero to merge the translations we currently have and fix the sync code to properly handle the manual pages?

### Synchronization

The updated POT file is automatically fetched by Transifex from the `master` branch of our GitHub repository.

Updated PO files with translations are merged back into the repository during the [release process](release-process.md).

### Transifex client

As of 2025, Fedora 41 unfortunately only ships the old client (`transifex-client`), which is no longer useful.  The new client is available in [binary form from GitHub](https://github.com/transifex/cli/releases) (download, unpack and copy to `~/bin`). To complete the configuration, fill in your API token:

```ini title="~/.transifexrc"
[https://www.transifex.com]
rest_hostname = https://rest.api.transifex.com
token         = 1/***
```

To make sure that Transifex client is installed correctly, try to run it with the following command:

```shell
$ tx --help
```

### Trying it out

To check updated translations locally, you need to build `mc` from source. Check out the repository and run the following command to fetch the translations from Transifex:

```shell
$ ./maint/sync-transifex/po-from-transifex.py
```

Then build and install as usual.
