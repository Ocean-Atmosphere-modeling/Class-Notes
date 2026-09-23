# Git on Amarel: what each command did

A walkthrough of a first session in the `gulf-of-guinea` repo on Amarel: pull, branch, edit, stage, commit and push.

## How your work moves

| GitHub | Working folder | Staging area | Commit |
|---|---|---|---|
| the shared copy (origin) | files you edit | changes queued with `add` | a saved snapshot |

`git pull` brings changes down from GitHub. `git add` moves edits into the staging area. `git commit` saves them as a snapshot on your machine. `git push` sends the snapshot to GitHub.

## `git pull`

Downloads the latest changes from GitHub and merges them into the branch you are on.

```
$ git pull
Enter passphrase for key '/home/bh535/.ssh/id_ed25519_github':
From github.com:Ocean-Atmosphere-modeling/gulf-of-guinea
 * [new branch]      becca      -> origin/becca
Already up to date.
```

- `Enter passphrase`: your SSH key has a passphrase, so Git asks for it each time it talks to GitHub.
- `[new branch] becca -> origin/becca`: GitHub has a branch called `becca` that your copy had not seen yet. It now knows about it.
- `Already up to date`: you were on `main`, and it had no new changes.

## `git status`

Shows which branch you are on and whether any files have changed. It changes nothing, so run it as often as you like.

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

- `On branch main`: the branch you are working on right now.
- `working tree clean`: you have not edited anything since the last commit.

## `git checkout becca`

Switches to the `becca` branch. A branch is your own line of work, so your edits stay separate from `main` until you choose to merge them.

```
$ git checkout becca
branch 'becca' set up to track 'origin/becca'.
Switched to a new branch 'becca'
```

- `set up to track 'origin/becca'`: your local `becca` is linked to the one on GitHub, so `git pull` and `git push` know where to go.
- "New branch" here means new on your copy of the repo. It already existed on GitHub.

## `ls`

Not a git command. It lists the files in the current folder.

```
$ ls
LICENSE  README.md
```

These are the two files the repo starts with.

## `nano test.md`

Not a git command either. It opens the nano text editor and creates `test.md` if it does not exist. Type some text, then save with Ctrl+O, Enter and exit with Ctrl+X.

```
$ nano test.md
```

Anything after `#` on a command line is a comment. The shell ignores it, so it is a handy place for notes to yourself.

## `git add .`

Queues your changes for the next commit. The `.` means everything in the current folder that is new or changed.

```
$ git add .
```

- It prints nothing when it works.
- To queue one file only, use `git add test.md`.

> [!NOTE]
> `add` puts changes in the **staging area**, a holding spot inside your repo. It is not a folder, and it is not the same thing as your `becca` branch. The branch is where your commits are saved.

## `git status` (again)

Run it after `add` to confirm what is queued.

```
$ git status
On branch becca
Your branch is up to date with 'origin/becca'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   test.md
```

- `On branch becca`: the checkout worked.
- `Changes to be committed`: `test.md` is staged and ready.
- `git restore --staged test.md` removes it from the queue if you change your mind.

## `git commit -m 'created a test file'`

Saves a snapshot of everything staged, with a short message describing it. `-m` means the message follows in quotes. Without it, Git opens an editor and asks for one.

```
$ git commit -m 'created a test file'
[becca a3f7aa9] created a test file
 Committer: Becca Horwitz <bh535@amarel3.amarel.rutgers.edu>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
...
 1 file changed, 1 insertion(+)
 create mode 100644 test.md
```

- `[becca a3f7aa9]`: the commit was saved on the `becca` branch. `a3f7aa9` is the commit's short ID.
- `1 file changed, 1 insertion(+)`: one file, one new line. `create mode 100644` means `test.md` is a new, regular file.
- The long message about your name and email is a notice, not an error. The commit worked. Git guessed your identity from your Amarel username and host. The next section shows how to set it properly.
- A commit is saved on your machine only. GitHub does not have it until you push.

## Fix your name and email for future commits

Git stamps every commit with a name and email. Set them once on Amarel, using the same email as your GitHub account so commits are linked to you.

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
git config --global --list
```

- The last command prints your settings so you can check them.
- A commit that is already on GitHub keeps the identity it was made with. Changing it would rewrite pushed history and need a force push, so for a test commit, leave it. The new settings apply from the next commit on.

## `git push`

Sends your commits on `becca` to GitHub.

```
$ git push
Enter passphrase for key '/home/bh535/.ssh/id_ed25519_github':
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 64 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 326 bytes | 163.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To github.com:Ocean-Atmosphere-modeling/gulf-of-guinea.git
   7bf658e..a3f7aa9  becca -> becca
```

- `Enter passphrase`: the SSH key passphrase again, as with `git pull`.
- The `Enumerating`, `Counting`, `Compressing` and `Writing objects` lines are Git packing your new commit and uploading it. Nothing to do here.
- `7bf658e..a3f7aa9  becca -> becca`: the `becca` branch on GitHub moved from commit `7bf658e` to your new commit `a3f7aa9`. Left of the arrow is your local branch, right is the branch on GitHub.
- Plain `git push` worked because `becca` already tracks `origin/becca`, which `git checkout becca` set up.
- Your work is on `becca`, and `main` is untouched. On GitHub, you can open a pull request to merge `becca` into `main` when you are ready.

## The daily loop

Everything above, in the order you will repeat it:

```bash
git pull
nano yourfile.py
git add .
git commit -m 'what changed'
git push
```

- Run `git status` any time to see where you are.
- Write commit messages that say what changed, like `add MCA plotting script`.
