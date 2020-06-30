When you edit a previous commit using `rebase`, the changes that you make in that WILL NOT be reflected in the branches that contain (reach to) that commit in their history. That is, let's say that you have the following commit graph:

```
A - B - C - D <- master
            |
            - E <- branch_1
            |
            - F <- branch_2
```

When you edit, say commit B using `git rebase -i A`, the commit graph WILL NOT be as following:

```
A - B' - C - D <- master
             |
             - E <- branch_1
             |
             - F <- branch_2
```

(B' indicates the new (edited) version of B)

Instead, it will be as follows:

```
A - B' - C - D <- master
|
 -  B  - C - D - E <- branch_1
             |
              -  F <- branch_2
```

In order the make the graph like the second graph (the one you initially expected it to be after the rebase), you have to perform "rebase onto" on _each branch that you want_. In this example, executing the following commands will be sufficient to make the graph be like the second graph:

```
git checkout branch_1
git rebase --onto master HEAD^
git push -f
git checkout branch_1
git rebase --onto master HEAD^
git push -f
```
References
==========
https://stackoverflow.com/questions/3810348/setting-git-parent-pointer-to-a-different-parent
