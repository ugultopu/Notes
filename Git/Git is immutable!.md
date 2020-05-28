When you "rewrite history" in Git (such as amending a commit, rebasing, etc.), what actually happens is that you are NOT rewriting the history. Instead, the old history stays as-is, and Git creates a "new history" out of the old one that contains the changes that you want, and Git moves the pointers around (branches, etc.) to "make it look like" the it has rewritten the history.

Git Snapshots
=============
Git thinks about data as "snapshots". So, when you say "git add", Git says "I wanna add the _content_ in this file to my next commit", instead of thinking like "putting a _file_ into version control". Hence, contrary to other version control systems (like SVN), you run "git add" _every time you make a change to the file_. Because what Git does is that it simply takes "snapshots of the whole repository in each commit", and "git add" tells Git what content to include in each snapshot. Whereas in for example SVN, "add" means "start tracking this file in the version control system. That is, start observing and recording the _changes to this file_". Git does not think in this way. Git treats every commit _as a snapshot_ of the whole repository.

[How "git add _file_" works?](https://youtu.be/ZDR433b0HJY?t=909)
---------------------------

- Git takes the content of the file. Note that Git takes the whole content of the file, not just a _delta compared to the previous version_, etc.
- Ignores the filename.
- Checksums this content with a SHA-1 hash.
- Puts the content (not the checksum, not the result of the SHA-1 hash, the content itself) in its (Git's) database. Git's database is basically nothing but a key-value store.
- Gives you back the SHA-1 hash (the checksum) as the key to Git's database (the key-value store). The SHA-1 hash is the hash that has been calculated two steps before. This way, when we make a query on Git's database (the key-value store) using this key (the checksum, the SHA-1 hash), Git will give us back the value that corresponds to this SHA-1 hash, which is the whole content of the file named _file_, which has been added with the `git add file` command.

Source: This [awesome video][Scott Chacon Video Presentation] presentation by Scott Chacon.

[Scott Chacon Video Presentation]: https://youtu.be/ZDR433b0HJY?t=629
