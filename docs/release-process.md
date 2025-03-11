# Release process

* We usually make two releases per year (spring and fall)
* We currently use a semantic versioning scheme (`4.8.XX`)

## Pre-requisites

### Release VM

- Install a fresh Fedora VM to cut a release:
```shell
yum build-dep mc
yum install git-core fakeroot check-devel po4a libX11-devel
```
- Configure `git`:
```shell
git config --global user.name "Yury V. Zaytsev"
git config --global user.email "yury@shurup.com"
```
- Port over the GPG keyring to make it possible to sign the tag.

### Transifex

[Set up Transifex](translations.md) to be able to work with translations.

### GPG

(see before: [wiki:GpgSetUpForSigningReleases how to set up your gpg key for signing releases]) # TODO

## Release process

[Copy and paste]({{ page.edit_url | replace("/mc", "/website") }}) the following list into the `Prepare for release mc-4.8.XX` [issue on GitHub]({{ config.repo_url }}/issues):

### Pre-release tasks

- [ ] Prepare the repository for release:
```shell
git clone git@github.com:MidnightCommander/mc.git
git fetch
git checkout master
git reset --hard origin/master
git clean -dfx
./autogen.sh
mkdir dist; cd dist; ../configure; cd ..
```
- [ ] Download PO translations from Transifex:
```shell
./maint/sync-transifex/po-from-transifex.py
```
- [ ] Commit PO translations to `git`:
```shell
make -C dist/po update-po
git add po/*.po
git commit -s -m 'maint: update PO translations from Transifex'
git push origin master
```
- [ ] Download hints translations from Transifex:
```shell
./maint/sync-transifex/hints-from-transifex.py
```
- [ ] Commit hints translations to `git`:
```shell
git add doc/hints/l10n/mc.hint.*
git commit -s -m 'maint: update hints translations from Transifex'
git push origin master
```
- [ ] Create a new `NEWS-4.8.YY` [wiki page]({{ config.repo_url }}/wiki) for the next version with an empty template. The template can be copied from the current `NEWS` wiki page (without the list of tasks and bug reports).
- [ ] Add the content of the current `NEWS` wiki page to the `doc/NEWS` file in the `git` repo:
```shell
git add doc/NEWS
git commit -s -m 'maint: update doc/NEWS file'
git push origin master
```
- [ ] Create a new version label[^1] on GitHub (`ver: 4.8.XX`)
- [ ] Create a new milestone[^1] on GitHub (`4.8.YY`)

## Cutting release tarballs

- [ ] Create a new `git` tag:
```shell
git tag -s 4.8.XX  # "Release" or "RCn"
```
- [ ] Create `*.tar.(bz2|xz)` distribution archives:
```shell
cd dist; fakeroot make dist-bzip2 && fakeroot make dist-xz

# https://bugzilla.redhat.com/show_bug.cgi?id=2338285
cat mc-4.8.33.tar | XZ_OPT=${XZ_OPT--e} xz -c >mc-4.8.33.tar.xz
```
- [ ] Compute checksums for distribution archives:
```shell
sha256sum mc-*tar.* > mc-4.8.XX.sha256
```
- [ ] Upload source packages and checksums to the issue
- [ ] Developers should download tarballs, verify checksums, compile and install locally; if everything is OK, developers should vote for the release.
```shell
./configure --prefix=$(pwd)/install
make install
make check
```
- [ ] Push out the release (or release candidate) tag:
```shell
git push origin 4.8.XX
```
- [ ] Upload source packages and checksums to the mirror master (maintainers should have access via public keys):
```shell
scp mc-4.8.XX.* midnightcommander@ftp-osl.osuosl.org:/home/midnightcommander/data
ssh midnightcommander@ftp-osl.osuosl.org 'ls -als /home/midnightcommander/data/' | grep 4.8.3
```
- [ ] Trigger distribution of the tarballs to the mirrors:
```shell
ssh midnightcommander@ftp-osl.osuosl.org '/home/midnightcommander/trigger-midnightcommander'
```
- [ ] Check that files can be downloaded; adjust permissions (`0644`) if necessary

### Post-release tasks

- [ ] Update the home page with the latest release version
- [ ] Write an announcement highlighting user-visible changes
- [ ] Create a new issue `Prepare for release mc-4.8.YY` (type: `Task`, label: `area: adm`) for the **next** release
- [ ] Close issue for **current** release
- [ ] Close **current** milestone[^1]

[^1]: Labels and milestones are managed via Pulumi
