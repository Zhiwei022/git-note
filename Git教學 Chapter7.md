# Git教學 Chapter7

:::info
本文章是依據 "為你自己學Git--高見龍著" 這本書的內容所寫的。
:::

###### tags: `Git教學`
[TOC]

## 7.1 (狀況題) 修改歷史紀錄
**1. 用指令** (困難)
看書本P.180

**2. 用sourcetree** (簡單)
對某個 Commit 按 Rebase children of SHA-1 interactively，再對想修改的 Commit 訊息按 Edit Message。

![](https://i.imgur.com/EFx5rBU.png)

![](https://i.imgur.com/fzaVWeB.png)


## 7.2 (狀況題) 把多個 Commit 合併成一個 Commit
**1. 用指令** (困難)
看書本P.185

**取消剛剛這次的Rebase**
```git
$ git rebase ORIG_HEAD --hard
```

**2. 用sourcetree** (簡單)
對某個 Commit 按 Rebase children of SHA-1 interactively，再對想修改的 Commit 訊息按 **Square with previous commit**，把前一個 Commit 往上收。最後再對合併的 Commit 按 **Edit Message**。合併會長出新的 Commit。
![](https://i.imgur.com/EFx5rBU.png)

![](https://i.imgur.com/xpndPtu.png)

![](https://i.imgur.com/xMXLGjI.png)


## 7.3 (狀況題) 把一個 Commit 拆成多個 Commit
**1. 用指令 (sourcetree 沒有支援)**
看書本P.190
最好是把拆 Commit 裡有提交多個檔案的那種比較好。
若是只有1個檔案的，則有拆跟沒拆一樣。


## 7.4 (狀況題) 在某些 Commit 中再加入新的 Commit
**1. 用指令 (sourcetree 沒有支援)**
```git
$ git log --oneline
399612e (HEAD -> master) add dog1.c dog2.c
f134afb add cat1.c cat2.c
ac120a5 update test.txt
6baae99 update test.txt
b8e3153 add .gitignore
d77fa5d add new folder images
30b5dc8 add and update hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit


# 從一個 parent Commit 往後修，然後指定(edit)要從哪個 Commit 後開始加，如 add dog 和 add cat 之間。
$ git rebase -i d6bc128
pick 668d1de add test.txt
pick e2af572 update test.txt
pick 8ff5fef update test.txt
pick 30b5dc8 add and update hello.c
pick d77fa5d add new folder images
pick b8e3153 add .gitignore
pick 6baae99 update test.txt
pick ac120a5 update test.txt
edit f134afb add cat1.c cat2.c
pick 399612e add dog1.c dog2.c

# Rebase d6bc128..399612e onto d6bc128 (10 commands)
#
# Commands:
# p, pick <commit> = use commit

# 顯示在當前HEAD在哪
$ git log --oneline
f134afb (HEAD) add cat1.c cat2.c
ac120a5 update test.txt
6baae99 update test.txt
b8e3153 add .gitignore
d77fa5d add new folder images
30b5dc8 add and update hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit

# 加入新 Commit
$ touch cat3.c
$ git add cat3.c
$ git commit -m "add cat3.c"

$ touch cat4.c
$ git add cat4.c
$ git commit -m "add cat4.c"

# 達成任務後，結束rebase
$ git rebase --continue
Successfully rebased and updated refs/heads/master.

# 查看當前所有 Commit
$ git log --oneline
3f24f75 (HEAD -> master) add dog1.c dog2.c
5c6be62 add cat4.c
f898c09 add cat3.c
f134afb add cat1.c cat2.c
ac120a5 update test.txt
6baae99 update test.txt
b8e3153 add .gitignore
d77fa5d add new folder images
30b5dc8 add and update hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit
```


## 7.4 (狀況題) 調整順序或刪除 Commit
### 調整順序 Commit
**1. 用指令 (麻煩)**
看書本P.197

**2. 用sourcetree (方便)**
對某個 Commit 按 Rebase children of SHA-1 interactively，再用滑鼠拖拉順序，或是用視窗的上下方按鈕調整，確定後按OK。

![](https://i.imgur.com/EFx5rBU.png)

![](https://i.imgur.com/z3brzV9.png)

### 刪除 Commit
**1. 用指令 (麻煩)**
看書本P.200

**2. 用sourcetree (方便)**
對某個 Commit 按 Rebase children of SHA-1 interactively，再對目標 Commit 按右鍵，選 **Delete Commit**，確定後按OK。 

![](https://i.imgur.com/EFx5rBU.png)

:::danger
使用 **rebase** 指令時一定要小心，因為其他 Commit 可能跟要做修改的 Commit 有**相依性**。
:::

## 7.6 Reset、Revert、Rebase指令有什麼差別
### Reset 指令使用
**1. 用指令**
```git
$ git log --oneline
7e9de43 (HEAD -> master) add dog3.c
3f24f75 add dog1.c dog2.c
5c6be62 add cat4.c
f898c09 add cat3.c
f134afb add cat1.c cat2.c
ac120a5 update test.txt
6baae99 update test.txt
b8e3153 add .gitignore
d77fa5d add new folder images
30b5dc8 add and update hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit

# 想要取消 add dog3.c
$ git revert HEAD --no-edit
[master e095dcf] Revert "add dog3.c"
 Date: Tue Mar 21 14:04:30 2023 +0800
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 dog3.c

# 回到 Revert前的狀態
$ git revert HEAD^ --hard

```
Revert 後，之前不要的 Commit **還是存在**，只是**多一個新的** Commit ，來取消當初不要的 Commit。

![](https://i.imgur.com/4BMmgV6.png)

**2. 用sourcetree**
直接在不要的 Commit 按右鍵。

![](https://i.imgur.com/WjJWOXV.png)

![](https://i.imgur.com/dHkydKP.png)


:::info
**什麼時候用 Revert 指令?**
若是**單人**作業則用 Reset 就好。
但若是**多人**協同時，用 Revert 比較，誰取消 Commit 也很清楚。
:::

| 指令   | 修改歷史紀錄 | 說明                                                                                                                                                                                                       |
| ------ | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Reset  | O            | 把目前的狀態設定成某個指定的 Commit 狀態，通常適用於尚未推出去的 Commit                                                                                                                                    |
| Rebase | O            | 不管新增、修改、刪除 Commit 都非常方便，用來整理、編輯還沒有推出去的 Commit 相當方便，但通常也只適用於尚未推出去的 Commit                                                                                  |
| Revert | X            | 新增一個 Commit 來取消另一個 Commit 的內容，原本的 Commit 依舊保存於歷史紀錄中。雖然會因此增加 Commit 數，但通常比較適用於已經推出去的 Commit ，或是不允許使用 Reset 或 Rebase之修改歷史紀錄的指令的場合。 |
