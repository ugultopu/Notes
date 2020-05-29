When you "rewrite history" in Git (such as amending a commit, rebasing, etc.), what actually happens is that you are NOT rewriting the history. Instead, the old history stays as-is, and Git creates a "new history" out of the old one that contains the changes that you want, and Git moves the pointers around (branches, etc.) to "make it look like" the it has rewritten the history.

Git Snapshots
=============
Git thinks about data as "snapshots". So, when you say "git add", Git says "I wanna add the _content_ in this file to my next commit", instead of thinking like "putting a _file_ into version control". Hence, contrary to other version control systems (like SVN), you run "git add" _every time you make a change to the file_. Because what Git does is that it simply takes "snapshots of the whole repository in each commit", and "git add" tells Git what content to include in each snapshot. Whereas in for example SVN, "add" means "start tracking this file in the version control system. That is, start observing and recording the _changes to this file_". Git does not think in this way. Git treats every commit _as a snapshot_ of the whole repository.

[How "git add _file_" works?](https://youtu.be/ZDR433b0HJY?t=909)
-----------------------------------------------------------------
- Git takes the content of the file. Note that Git takes the whole content of the file, not just a _delta compared to the previous version_, etc.
- Ignores the filename.
- Checksums this content with a SHA-1 hash.
- Puts the content (not the checksum, not the result of the SHA-1 hash, the content itself) in its (Git's) database. Git's database is basically nothing but a key-value store.
- Gives you back the SHA-1 hash (the checksum) as the key to Git's database (the key-value store). The SHA-1 hash is the hash that has been calculated two steps before. This way, when we make a query on Git's database (the key-value store) using this key (the checksum, the SHA-1 hash), Git will give us back the value that corresponds to this SHA-1 hash, which is the whole content of the file named _file_, which has been added with the `git add file` command.

[How "git commit" works?](https://youtu.be/ZDR433b0HJY?t=958)
-------------------------------------------------------------
- When you say "git commit", Git records a _manifest_. This manifest is called a "tree", a "tree object" by Git. A "tree" is nothing but a manifest. This manifest is sort of like a "directory listing". The manifest contains:
  - The filenames of each file that are in the staging are right before this commit. That is, the filenames of each file that has been added to the staging area.
  - The checksums as the keys to objects in Git's database. These objects contains the contents that are referred to by the keys (which are checksums).
  - The keys (the checksums) and file names are matched and put into the manifest.
- The manifest is added to Git's database (the key-value store) and a checksum for the manifest has been calculated. Since the manifest is just like any other file, the checksum of the manifest is calculated.
- Additional metadata about the commit, such as date-and-time, commit author data (name and email, etc.) are added.
- The pointer to (the checksum of) the parent commit is included to be able to navigate the commit history.
- This whole thing has been added to Git's database (the key-value store) and another checksum for this whole thing has been calculated.
- This final checksum is _the checksum of this commit_. That is, this final checksum is the checksum that we (anybody) refers to this commit from now on.

[What happens when you _change a file_ in Git?](https://youtu.be/ZDR433b0HJY?t=1043)
------------------------------------------------------------------------------------
As we have stated, there is no concept of "file" in Git. That is, Git does not "version a file". Git versions "a whole repository". That is, there is no concept of version 1, version 2, etc. "for a file".

So, if we change a file, Git will see different content. This new (changed) content will checksum differently (that is, the SHA-1 hash of this new (and different) content will be different than the previous version). Hence, if we want to add this new version of this file, Git will simply:

- Put the _whole content of this file_ to Git's database (the key-value object database).
- Calculate a checksum _for the whole content of this file_.

**That is, there will be two separate entries (objects) in Git's database with almost the exact same contents (except the changes that we made)**. Only thing is that they will checksum differently, since they will have a minor difference (which is why we are adding the file to Git's staging area after the changes in the first place).

![Manifest after changing a file][Manifest after changing a file]

So, again, **every single commit in Git is a SNAPSHOT OF THE WHOLE REPOSITORY**. Commits are not "diffs" or whatever. Every single commit is a proper whole snapshot of all the content of all the files in the repository. The diffs, etc. are calculated _on the fly_ by comparing the selected commits and finding the diffs _between the commits_.

Source: This [awesome video][Scott Chacon Video Presentation] presentation by Scott Chacon.

[Scott Chacon Video Presentation]: https://youtu.be/ZDR433b0HJY?t=629
[Manifest after changing a file]: manifest-after-changing-a-file.png
