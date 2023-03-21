# Git教學 Chapter6

:::info
本文章是依據 "為你自己學Git--高見龍著" 這本書的內容所寫的。
:::

###### tags: `Git教學`
[TOC]

## 6.1 為何要使用 branch ?
在新增新功能，修正 Bug，或是實驗某看法時，可用 branch 來進行，待確認沒有問題後再合併回來，不影響 master 的運行狀況。

Git 使用 branch 的成本非常低，非常方便。

## 6.2 開始使用 branch
查看當前有哪些分支
```git
# 如果 git branch 後面沒有接任何參數，則僅會印出有哪些分支
# Git 會預設一個 master 分支，前面的星號*表示正在這個分支上
$ git branch
* master
```

新增分支
```git
# 在 git branch 後面接想要的分支名稱
$ git branch cat
$ git branch dog

$ git branch
  cat
  dog
* master
```

分支改名子
```git
# 把分支改名子，用的是 -m 參數
$ git branch -m cat tiger

$ git branch
  dog
* master
  tiger

# 也可以更改預設分支master
$ git branch -m master slave
  dog
* slave
  tiger
```

刪除分支
```git
# 把分支刪除，用的是 -d 參數
# 如果要刪的分支還沒完全合併， -d 會不給刪，則需要用 -D 參數強制刪除
$ git branch -d dog
Deleted branch dog (was e2710a9).

$ git branch
* slave
  master
```

切換分支
```git
# 使用 checkout 來切換
$ git checkout cat
Switched to branch 'cat'

$ git branch
* cat
  dog
  master
```

在分支上做更動，不影響其他分支
```git
# 新增2個.c檔案到 cat 分支
$ git checkout cat

$ touch cat1.c
$ git add cat1.c

$ touch cat2.c
$ git add cat2.c

$ git commit -m "add cat1.c cat2.c"

$ git log --oneline
795794e (HEAD -> cat) add cat1.c cat2.c
e2710a9 (master, dog) update test.txt
29912da update test.txt
b5db030 add .gitignore
76c1cc0 add new folder images
e95f32f update hello.c
cb29e1f update hello.c
4a0045b add hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit

$ ls -al
總用量 32
drwxrwxr-x 4 wayne wayne 4096  3月 20 14:37 .
drwxr-xr-x 4 wayne wayne 4096  3月 20 00:07 ..
-rw-rw-r-- 1 wayne wayne    0  3月 20 14:37 cat1.c
-rw-rw-r-- 1 wayne wayne    0  3月 20 14:37 cat2.c
drwxrwxr-x 8 wayne wayne 4096  3月 20 14:38 .git
-rw-rw-r-- 1 wayne wayne  203  3月 20 11:27 .gitignore
-rw-rw-r-- 1 wayne wayne  223  3月 20 01:35 hello.c
drwxrwxr-x 2 wayne wayne 4096  3月 20 01:43 images
-rw-rw-r-- 1 wayne wayne   12  3月 20 02:23 README.md
-rw-rw-r-- 1 wayne wayne  101  3月 20 11:27 test.txt

$ git checkout master
Switched to branch 'master'
# 會看不到原本新增的cat1.c和cat2.c

$ ls -al
總用量 32
drwxrwxr-x 4 wayne wayne 4096  3月 20 14:39 .
drwxr-xr-x 4 wayne wayne 4096  3月 20 00:07 ..
drwxrwxr-x 8 wayne wayne 4096  3月 20 14:39 .git
-rw-rw-r-- 1 wayne wayne  203  3月 20 11:27 .gitignore
-rw-rw-r-- 1 wayne wayne  223  3月 20 01:35 hello.c
drwxrwxr-x 2 wayne wayne 4096  3月 20 01:43 images
-rw-rw-r-- 1 wayne wayne   12  3月 20 02:23 README.md
-rw-rw-r-- 1 wayne wayne  101  3月 20 11:27 test.txt
```

若分支不存在，但想切過去，使用 -b 參數
```
$ git checkout -b sister
Switched to a new branch 'sister'
```

## 6.3 對分支的誤解
見書本 P.130~P.132
:::warning
分支就只是一個指標，一張貼紙，貼在某個 Commit 上面而已。
不是複製出另一個目錄給新的 branch 來指。
:::


### 如果更改到一半還沒commit時，就切換分支會怎樣？
:::info
Git 在切換分支時，會用該分支所指向的 Commit 的內容**更新**工作目錄（**Working Directory**）和暫存區（**Staging Area**）。
但在切換分支之前所作的修改（尚未 Commit）則還是會留在工作目錄和暫存區。
:::


```git
$ git checkout cat
Switched to branch 'cat'

$ touch cat3.c

$ git status
On branch cat
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	cat3.c

nothing added to commit but untracked files present (use "git add" to track)

$ git checkout master
Switched to branch 'master'

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	cat3.c

nothing added to commit but untracked files present (use "git add" to track)
```

## 6.4 合併分支
:::info
盡量以 master 作為主要分支，其他分支作為合併的對象
:::

```git
$ git checkout master
Switched to branch 'master'

$ git merge cat
Updating e2710a9..795794e
Fast-forward
 cat1.c | 0
 cat2.c | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 cat1.c
 create mode 100644 cat2.c

$ ls -al
總用量 32
drwxrwxr-x 4 wayne wayne 4096  3月 20 15:31 .
drwxr-xr-x 4 wayne wayne 4096  3月 20 00:07 ..
-rw-rw-r-- 1 wayne wayne    0  3月 20 15:31 cat1.c
-rw-rw-r-- 1 wayne wayne    0  3月 20 15:31 cat2.c
drwxrwxr-x 8 wayne wayne 4096  3月 20 15:31 .git
-rw-rw-r-- 1 wayne wayne  203  3月 20 11:27 .gitignore
-rw-rw-r-- 1 wayne wayne  223  3月 20 01:35 hello.c
drwxrwxr-x 2 wayne wayne 4096  3月 20 01:43 images
-rw-rw-r-- 1 wayne wayne   12  3月 20 02:23 README.md
-rw-rw-r-- 1 wayne wayne  101  3月 20 11:27 test.txt

$ git log --oneline
795794e (HEAD -> master, cat) add cat1.c cat2.c
e2710a9 (dog) update test.txt
29912da update test.txt
b5db030 add .gitignore
76c1cc0 add new folder images
e95f32f update hello.c
cb29e1f update hello.c
4a0045b add hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit
# 本來落後 1 個 Commit 的 master分支，在進行合併後，進度已經跟 cat 分支一樣。
```

### A合併B，跟B合併A有什麼不同?
以下為 cat 分支合併 dog 分支，新的 Commit 會以 cat 分支為主。

![](https://i.imgur.com/pj6PvLN.png)

## 6.5 （狀況題）為什麼我的分支沒有小耳朵?
如果 Commit 之間差異不大，則軟體會自動使用 fast-forward 來進行 merge，而不會出現小耳朵，也不會出現新的 Commit。
如果 Commit 之間差異很大，如上圖，會出現小耳朵，也會出現新的 Commit。
:::warning
若硬要出現小耳朵，則在終端機裡加入 --no-ff 參數，--no-ff 代表 no fast-forward。
若是用sourcetree，則使用上方的 merge 圖示，並勾選 "Create a commit even if merge resolved via fast-forward"。
:::

## 6.6 （常見問題）合併過的分支要留著嗎
分支就只是一個貼紙，要刪不刪皆可，取決於個人。
若是還沒合併過的分支，就不要刪。


## 6.7 不小心把還沒合併的的分支刪掉，還救的回來嗎?
可以，再建一個新的 branch，再指向原本的 Commit 即可。
```git
git branch new_cat 8056699
```
:::danger
分支只是指向某一個 Commit 的指標。
雖然分支消失了，但 Commit 不會跟著消失。
:::

## 6.8 另一種合併方式(使用rebase)
見書本P.154

## 6.9 合併發生衝突了，怎麼辦?
常發生於不同分支修改同個檔名，同一行程式碼。

若是一般merge發生衝突，在merge後會出現2個相同檔名的驚嘆號，必須要選擇 git add 哪一個分支的檔案。

若是rebase發生衝突，看書本P.166。

## 6.10 (冷知識) 為什麼Git開分支很便宜?
分支只是一個有40個字元的檔案而已，只是一個標籤。

## 6.11 (冷知識) Git怎麼知道現在在哪一個分支
目前分支位置紀錄在 .git 目錄裡的 HEAD 檔案裡面。
```git
$ git branch
  cat
  dog
* master

$ cat .git/HEAD
ref: refs/heads/master

$ git checkout cat

$ cat .git/HEAD
ref: refs/heads/cat
```

### (冷知識) HEAD也有縮寫喔
可以用@來替代HEAD
```git
$ git reset HEAD^
# 上下兩個相同功能
$ git reset @^
```

### ORIG_HEAD是什麼東西?
看書本P.174


## 6.12 (狀況題) 可以從過去的Commit再長出一個新的分支出來嗎?
當然可以!!

**對終端機來說:**
```git
$ git branch bird b5db030
# 或者
$ git checkout -b bird b5db030
```

**對sourcetree來說:**
![](https://i.imgur.com/kiiuRBg.png)



