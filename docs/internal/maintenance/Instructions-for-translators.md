# Instructions for translators

## Table of contents
* [Overview]
* [Software preparation]
* [GitHub preparation]
* [Tools]
* [Clone preparation]
* [Translation steps]
* [Updates to msgids]
* [Adding a new language]
* [Technical details]

## Overview

This document has instructions for translation in [PO files] and the
[Gettext system]. It is used in Zonemaster-Engine, Zonemaster-CLI and
Zonemaster-Backend. Zonemaster-GUI uses a different solution. See the
[Zonemaster GUI translation guide] for instructions for that part.

[PO files] are found in the following directories:
* [share][zonemaster-engine share] in Zonemaster-Engine
* [share][zonemaster-cli share] in Zonemaster-CLI
* [share][zonemaster-backend share] in Zonemaster-Backend

There is one PO file per language in each component. A PO file consists of
multiple entries. An entry consists of three parts. First an optional header
(lines starting with "#") which can have the "fuzzy" tag (more below), second
the *msgid* which is the untranslated text (in English in the case of Zonemaster)
that comes from the program code (Perl code in the case of Zonemaster), and the
third part is the *msgstr* which is the translated text. The task of the
translator is to create and update *msgstr*.

In many *msgid* there is one or several arguments within `{...}`, e.g.
`{ns_ip_list}`. The same arguments must be found in *msgid* and *msgstr*. The
value of the argument will be inserted at run time. The format of that value
depends on the argument name, and a specification for arguments used, mainly in
Zonemaster-Engine, can be found in [Arguments for test case messages].

## Software preparation

For the steps below you need to work on a computer with Git, Perl and Gettext.
Select what OS you want to work on from the list below. Other OSs will also work,
but you will have to find instructions elsewhere.

* Rocky Linux

  Install the following:
  ```
  dnf install git make perl-App-cpanminus perl-Try-Tiny
  cpanm Locale::PO
  ```

* Debian

  Install the following:
  ```
  apt install gettext git liblocale-po-perl
  ```

* FreeBSD

  Install the following:
  ```
  pkg install gettext-tools git-lite gmake p5-Locale-PO p5-Try-Tiny
  ```

* Ubuntu

  Install the following:
  ```
  apt install gettext git make liblocale-po-perl
  ```

## GitHub preparation

For full integration with Zonemaster translation you need a GitHub account
and forks of the Zonemaster repositories. If you do not have one you can easily
[create an account at GitHub][GitHub signup]. If you are not willing to create
one, contact the Zonemaster work group for instructions by sending an email to
"zonemaster@zonemaster.net".

To create the forks of the Zonemaster repositories
1. Go to [GitHub] and log in with your account.
2. Go to the [Zonemaster-Engine repository].
3. Press "Fork" in the top right corner (see "[Forking a repository]").
4. Repeat steps 2 and 3 for the the following repositories:
   * [Zonemaster-CLI repository]
   * [Zonemaster-Backend repository]

Make sure that your public *ssh* key is uploaded to GitHub
(see "[Adding a new SSH key to your GitHub account]") and that its private key
is available on the computer you are going to work from.


## Tools

The [PO file][PO files] can be edited with a plain text editor, but then it is
important to keep the database structure of the file. There are tools that
makes editing of the PO files easier. When using those, the PO file is
handled as a database instead of as a plain file.

* There is [Emacs PO-mode], available as a plug-in to the [GNU Emacs] file
  editor, which makes updating and searching in the ".po" file easier and more
  robust.
* There is also "[GNOME Translation Editor]", a graphical PO editor
  available for at least Windows and Linux.
* There is [POEDIT] in free version that Zonemaster translator has reported to
  work well.
* There are more tools available, either cloud services or programs
  for download, and they could be found by searching for "po editor".


## Clone preparation

You need a local clone of the repository to work in (see
"[Cloning a repository]"). You need that for all repositories with messages to
translate -- Zonemaster-Engine, Zonemaster-CLI and Zonemaster-Backend. (For
Zonemaster-GUI see [Zonemaster GUI translation guide].)

Here it is assumed that the three repositories are cloned from the same
directory, i.e. all three clones are sub-directories to the same directory.

* If you use "old" clones, instead of creating them afresh, then you must do the
  following (if there is no `develop` branch, checkout `origin/develop` and skip
  `pull`).

  ```
  git fetch --all
  git checkout develop
  git pull
  ```

* Clone the three repositories:
  ```sh
  git clone https://github.com/zonemaster/zonemaster-engine
  git clone https://github.com/zonemaster/zonemaster-cli
  git clone https://github.com/zonemaster/zonemaster-backend
  ```

* Check out `origin/develop` branch in the three repositories:
  ```sh
  git -C zonemaster-engine checkout origin/develop
  git -C zonemaster-cli checkout origin/develop
  git -C zonemaster-backend checkout origin/develop
  ```

You must always start from `develop` branch (or `origin/develop` branch) and any
`develop` branch must be even with `origin/develop`.

Now it is time to connect your own forks created in [GitHub preparation] above
to the created clones. We take *Zonemaster-Engine* as an example, but should be
repeated for all clones.

* You have a user name at GitHub. Here we use "xxxx" as your user name and also
  the name of the remote in clone on the local machine.
* Enter the clone:
  ```
  cd zonemaster-engine
  ```

* Create the connection to your fork by making it "remote" and synchronize to
  it:
  ```
  git remote add xxxx git@github.com:xxxx/zonemaster-engine.git
  git fetch --all
  ```

Now you should have updated local clones of all three repositories.


## Translation steps

The steps in this section will cover most translation work. The steps are the
same for all three repositories.

* Make sure that the working tree is clean. Remove all other files and updates.
  If you have edited your PO file, then make a copy of it outside the repository.
  ```
  git clean -dfx
  git reset --hard
  ```

* The following command should report that the working tree is clean.
  ```
  git status --ignored
  ```

* Check-out the *develop* branch and create a new branch to work in. You can call
  the new branch whatever you want, but here we use the name
  "translation-update". If that name is already taken, you have to give it a new
  name or remove the old branch.
  ```
  git checkout origin/develop
  git checkout -b translation-update
  ```

* Go to the *share* directory. Stay in the *share* directory for the remaining
  translation steps.
  ```
  cd share
  ```

* If you have a partly edited PO file that you want to continue on, then it is
  time to copy it back into the `share` directory before the next step. Or use
  the PO file already in the `share` directory.

* Run the update command for the PO file for the language you are going to work
  with. Replace "xx" with the language code in question. This will synchronize
  the PO file with the *msgids* in the Perl code.
  ```
  ./update-po xx.po
  ```

* The PO file is updated with new *msgids*, if any, and now you can start
  working with it. Unless this is the first translation you only have to work
  with updated or untranslated items.

* Update the PO file with the tool of your choice (see section "[Tools]" above).
  You can copy the PO file to another computer, edit it there, and then copy it
  back to your Zonemaster-Engine clone -- or Zonemaster-CLI clone or
  Zonemaster-Backend clone depending on what repository you do the translation
  for.

* When doing the update, do not change the *msgid*, only the *msgstr*. The
  *msgid* cannot be updated in this process. They are the links between the Perl
  module and the PO file.

  * If you find a *msgid* that needs an update please see section
    [Updates to msgids] for how to report it.

* If there is a `{...}` in the *msgid* then [Arguments for test case messages]
  will help with the type of data if not obvious.

* Try to translate as close to the original as possible, but still make sure
  that the language is sound and valid.

* Inspect every *fuzzy entry* (tagged with "fuzzy"). Update *msgstr*
  if needed and remove the "fuzzy" tag. The "fuzzy" tag must always be removed.

* Search for *untranslated entries* (empty *msgstr*) and add a
  translation.

  * At the end of the file there could be *obsolete entries* (lines starting with
    "#~") and those could have matching translations, especially of the *msgid*
    has been changed.

* Any remaining *obsolete entries* (lines at the end of the file starting
  with "#~") could be removed. They serve no purpose anymore.

* With the following command, check that the messages arguments (`{...}`) in all
  *msgstr* strings match up with those in the *msgid* strings.
  ```
  ../../zonemaster-engine/util/check-msg-args xx.po
  ```

* When the update of all *msgstr* is complete run the `tidy-po` command to
  create a consistent formatting of the PO file. Do this again if you do
  further updates.

  ```
  make POFILES=xx.po tidy-po
  ```

* You can check all updates that have been done to the xx.po file. That includes
  both updates done by the update command and your updates.
  ```
  git diff xx.po
  ```

* When the update is completed, it is time to commit the changes. You should
  only commit the "xx.po" file.
  ```
  git commit -m 'Write a description of the change' xx.po
  ```

* There could be other files changed or added that should not be included.
  Run the status command to see them.
  ```
  git status
  ```

* Other changed files could be reset by a `git checkout`. This could also
  be done before creating the commit.
  ```
  git checkout FILENAME
  ```

* Files that were created but are not needed can just be removed. This could also
  be done before the commit.
  ```
  rm FILENAME
  ```

* Now push the local branch you created to your fork at GitHub.
  "translation-update" is name of the branch you created above and
  have committed the updates to. Use your GitHub user name instead of
  "xxxx".
  ```
  git push -u xxxx translation-update
  ```

* Go to your fork collection at GitHub, https://github.com/xxxx/ using your
  GitHub user name instead of "xxxx". There you can select the fork for this
  translation.

* Select to create a new pull request. Here we use Zonemaster-Engine as an
  example.
  * The base repository should be *zonemaster/zonemaster-engine*.
  * The base branch should be *develop* (not *master*).
  * The "head" should be your fork.
  * The "compare" should be the same branch as you created above and pushed to
    your fork, e.g. "translation-update".

* Inspect what GitHub says that will change by the pull request. It should
  only be the PO file that you have updated and nothing else. If additional
  files are listed, please correct or request for help.

  * If you by mistake have not started with an updated version of the develop
    branch you can get extra files listed.

* If you want to correct something now before the pull request has been created,
  do that in your local clone and do a new commit and push that to your fork.

* Press "create pull request", write a nice description and press "create"
  again.

* If you go back to your own computer and just keep the clone as it is, you
  can easily update the pull request if needed with more changes to the same
  PO file. When the pull request has been merged by the Zonemaster work group,
  you can delete the local clone and on your GitHub fork you can remove the
  branch. Or keep them for next time.


## Updates to msgids

The *msgid* cannot be updated in the translation process. If you find an *msgid*
that needs an update you can provide that information to the project. The
preferred way is to create an issue in the relevant repository.

* Message *(msgid)* in Zonemaster-Engine: [issue][new issue zonemaster-engine]
* Message *(msgid)* in Zonemaster-CLI: [issue][new issue zonemaster-cli]
* Message *(msgid)* in Zonemaster-Backend: [issue][new issue zonemaster-backend]

If the message is in Zonemaster-Engine then include the message tag found in the
header, e.g. "BASIC:NO_PARENT", in the issue description. Also include the *msgid*
as it is now and a suggestion for new wording.


## Adding a new language

If you want to add a new language, then follow steps in section
"[Translation steps]" above with some modifications. Before you add a language
contact the Zonemaster project to discuss timeplan and other aspects of the new
language. Every language should be updated at every new release of Zonemaster if
there are updated or added messages to be translated.

* First, as in the translation steps, create the branch to work in.
  ```
  git checkout origin/develop
  git checkout -b translation-update
  ```

There is no PO file for new language, and it has to be created. The easiest way
is to make a copy of an existing file.

* Determine what language code to use for the new language. It should be the
  correct [ISO 639-1] two-character code. By looking up the language on
  [Wikipedia] the correct code can be determined.

* It must be a code that is available in the *locale* system in the OSs that
  Zonemaster supports. Try the following commands to see if it is available.
  Replace "xx" with that code that you think it should be. Consult with
  Zonemaster Project if in doubt.
  ```
  locale -a | grep xx_      # Works in FreeBSD and Rocky Linux
  grep xx_ /etc/locale.gen  # Works in Debian and Ubuntu
  ```

* Go to the *share* directory and create a new blank PO file. Then "add" it
  to `git` before working on it.
  ```
  cd share
  make POLANG=xx new-po
  git add xx.po
  ```

* Verify and update the file header if needed.

* Now you go back to section "[Translation steps]" and continue in the same way
  as with an existing language.


## Technical details

The first step in updating the translations is to generate a new template file
("Zonemaster-Engine.pot"). In practice you rarely need to think about generating
it as it is generally performed as an implicit intermediate step.
If you do want to generate it, the command is `make extract-pot`.

The translated strings are maintained in files named "<LANG-CODE>.po". You will
find the current PO files in the "share" directories listed in the "[Overview]"
section. Translation and the languages are also presented in the main [README]
document.

The command `./update-po` will update the PO file with new message ids (*msgid*)
from the source code. Execute `./update-po xx.po` to update the PO file for
language "xx". This should only be strictly necessary to do when a module has
been added, changed or removed, but it it recommended to do this step every time.

Execute `make update-po` to update all the PO files with new message ids from the
source code.

By default the updated PO file will suggested translations for new message
ids based on fuzzy matching of similar strings. This is not always desirable
and you can disable fuzzy matching by executing one of the following
commands instead:
```
make update-po MSGMERGE_OPTS=--no-fuzzy-matching POFILES=xx.po
make update-po MSGMERGE_OPTS=--no-fuzzy-matching
```


<!-- Zonemaster links point on purpose on the develop branch. -->
[Adding a new SSH key to your GitHub account]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
[Adding a new language]:                       #adding-a-new-language
[Arguments for test case messages]:            ../../public/specifications/tests/ArgumentsForTestCaseMessages.md
[Clone preparation]:                           #clone-preparation
[Cloning a repository]:                        https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository#cloning-a-repository
[Emacs PO-mode]:                               https://www.gnu.org/software/gettext/manual/html_node/PO-Mode.html#PO-Mode
[Forking a repository]:                        https://docs.github.com/en/get-started/quickstart/fork-a-repo#forking-a-repository
[GNOME Translation Editor]:                    https://wiki.gnome.org/Apps/Gtranslator
[GNU Emacs]:                                   https://www.gnu.org/software/emacs/
[Gettext system]:                              https://www.gnu.org/software/gettext/
[GitHub preparation]:                          #github-preparation
[GitHub signup]:                               https://github.com/signup
[GitHub]:                                      https://github.com/
[ISO 639-1]:                                   https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[New issue zonemaster-backend]:                https://github.com/zonemaster/zonemaster-backend/issues/new
[New issue zonemaster-cli]:                    https://github.com/zonemaster/zonemaster-cli/issues/new
[New issue zonemaster-engine]:                 https://github.com/zonemaster/zonemaster-engine/issues/new
[Overview]:                                    #overview
[PO files]:                                    https://www.gnu.org/software/gettext/manual/html_node/PO-Files.html
[POEDIT]:                                      https://poedit.net/
[README]:                                      ../../../README.md
[Software preparation]:                        #software-preparation
[Technical details]:                           #technical-details
[Tools]:                                       #tools
[Translation steps]:                           #translation-steps
[Translation]:                                 https://github.com/zonemaster/zonemaster-engine/blob/develop/docs/Translation.pod
[Updates to msgids]:                           #updates-to-msgids
[Wikipedia]:                                   https://www.wikipedia.org/
[Zonemaster GUI translation guide]:            https://github.com/zonemaster/zonemaster-gui/blob/develop/docs/TranslationGuide.md
[Zonemaster-Backend repository]:               https://github.com/zonemaster/zonemaster-backend
[Zonemaster-CLI repository]:                   https://github.com/zonemaster/zonemaster-cli
[Zonemaster-Engine repository]:                https://github.com/zonemaster/zonemaster-engine
[Zonemaster-backend share]:                    https://github.com/zonemaster/zonemaster-backend/tree/develop/share
[Zonemaster-cli share]:                        https://github.com/zonemaster/zonemaster-cli/tree/develop/share
[Zonemaster-engine share]:                     https://github.com/zonemaster/zonemaster-engine/tree/develop/share




