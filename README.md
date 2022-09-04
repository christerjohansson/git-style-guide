# Git Style Guide

This is a Git Style Guide inspired by [*How to Get Your Change Into the Linux
Kernel*](https://kernel.org/doc/html/latest/process/submitting-patches.html),
the [git man pages](http://git-scm.com/doc) and various practices popular
among the community.

Translations are available in the following languages:

* [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [French](https://github.com/pierreroth64/git-style-guide)
* [German](https://github.com/runjak/git-style-guide)
* [Greek](https://github.com/grigoria/git-style-guide)
* [Italian](https://github.com/vincendep/git-style-guide)
* [Japanese](https://github.com/objectx/git-style-guide)
* [Korean](https://github.com/ikaruce/git-style-guide)
* [Polish](https://github.com/mbiesiad/git-style-guide/tree/pl_PL)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)
* [Russian](https://github.com/alik0211/git-style-guide)
* [Spanish](https://github.com/jeko2000/git-style-guide)
* [Swedish](https://github.com/christerjohansson/git-style-guide.git)
* [Thai](https://github.com/zondezatera/git-style-guide)
* [Turkish](https://github.com/CnytSntrk/git-style-guide)
* [Ukrainian](https://github.com/denysdovhan/git-style-guide)

Om du vill bidra till projektet, klicka på Fork för att kopiera projektet till ditt eget konto. Översätt texten och skapa sedan en Pull Request för att begära granskning och sammanfoga din text med ordinarie repository.

# Innehållsförteckning

- [Git Style Guide](#git-style-guide)
- [Innehållsförteckning](#innehållsförteckning)
  - [Grenar (Branches)](#grenar-branches)
  - [Genomförande (eng. Commits)](#genomförande-eng-commits)
    - [Messages](#messages)
  - [Merging](#merging)
  - [Misc.](#misc)
- [License](#license)
- [Credits](#credits)

## Grenar (Branches)

* Välj *korta* och *beskrivande* namn:

  ```shell
  # Bra
  $ git checkout -b oauth-migration

  # Dåligt - för vagt och generellt
  $ git checkout -b login_fix
  ```

* Identifierare från motsvarande ärenden i en extern tjänst (t.ex. en GitHub
  issue) är också bra exempel för användning i namn för grenar. Till exempel:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```
* Använd gemener i namn för grenar (eng. branch). Externa identifierare med versaler för öppna ärenden (issue) med versaler är ett giltigt undantag. Använd *bindestreck* för att separera ord.

  ```shell
  $ git checkout -b new-feature      # bra
  $ git checkout -b T321-new-feature # bra (Phabricator uppgifts-id)
  $ git checkout -b New_Feature      # dåligt
  ```

* När flera personer arbetar med *samma* funktion kan det vara bekvämt
  att ha *personliga* grenar (eng. branch) för funktioner och en *team-gemensam* gren för funktioner.
  Använd följande namngivningskonvention:

  ```shell
  $ git checkout -b feature-a/main # team-gemensam gren (eng. branch)
  $ git checkout -b feature-a/maria  # Marias personliga gren
  $ git checkout -b feature-a/nick   # Nicks personliga gren
  ```
  
  Slå samman de personliga grenarna efter behag till den team-gemensamma grenen (se ["Sammanslagning"](#merging)). Så småningom kommer den gemensamma grenen att slås samman till "main".

* Delete your branch from the upstream repository after it's merged, unless
  there is a specific reason not to.

* Ta bort grenen från det överordnade repositoriet när den har sammanfogats, såvida inte
  det finns en särskild anledning att inte göra det.

  Tips: Använd följande kommando när du befinner dig på grenen "main" för att lista sammanslagna
  grenar:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Genomförande (eng. Commits)

* Varje *commit* ska vara en enda *logisk ändring*. Gör inte flera
  *logiska ändringar* i en enskild *commit*. Till exempel, om en patch fixar ett fel och optimerar prestanda för en funktion, dela upp den i två separata *commits*.

  *Tips: Använd `git add -p` för att interaktivt förbereda specifika delar av dina
  modifierade filer.*

* Don't split a single *logical change* into several commits. For example,
  the implementation of a feature and the corresponding tests should be in the
  same commit.

* Commit *early* and *often*. Small, self-contained commits are easier to
  understand and revert when something goes wrong.

* Commits should be ordered *logically*. For example, if *commit X* depends
  on changes done in *commit Y*, then *commit Y* should come before *commit X*.

Note: While working alone on a local branch that *has not yet been pushed*, it's
fine to use commits as temporary snapshots of your work. However, it still
holds true that you should apply all of the above *before* pushing it.

### Messages

* Use the editor, not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything
  in a single line which usually results in non-informative, ambiguous commit
  messages.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* After that should come a blank line followed by a more thorough
  description. It should be wrapped to *72 characters* and explain *why*
  the change is needed, *how* it addresses the issue and what *side-effects*
  it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

  ```text
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  The pointers to your related resources can serve as a footer
  for your commit message. Here is an example that is referencing
  issues in a bug tracker:

  Resolves: #56, #78
  See also: #12, #34

  Source: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

* If a *commit A* depends on *commit B*, the dependency should be
  stated in the message of *commit A*. Use the SHA1 when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  also be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

## Merging

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "main" in order to merge it later.

  That said, *never rewrite the history of the "main" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/main
       # then merge
       ```

       This results in a branch that can be applied directly to the end of the
       "main" branch and results in a very simple history.

       *(Note: This strategy is better suited for projects with short-running
       branches. Otherwise it might be better to occassionally merge the
       "main" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_lightweight_tags)
  for personal use, such as to bookmark commits for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!
