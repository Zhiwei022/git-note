# Git教學 Chapter5

:::info
本文章是依據 "為你自己學Git--高見龍著" 這本書的內容所寫的。
:::

###### tags: `Git教學`
[TOC]


## Vim安裝
進入cmd執行下列命令安裝
```
$ sudo apt install vim
```

## Vim操作
Vim是Git編輯Commit訊息的預設編輯器

![](https://i.imgur.com/MmcMg7S.png)


## Git安裝
進入cmd執行下列命令安裝git
```
$ sudo apt install git
```
查看git裝在哪裡？
```
$ which git
```
查看git版本
```
$ git --version
```

## 設定Git (Git Configuration)
設定使用者名稱和Email，只須設定一次
```
$ git config --global user.name "Zhiwei022"
$ git config --global user.email "wayne64081143@gmail.com"
```

選擇commit時的編輯器
```
$ git config --global core.editor vim
```

設定顏色
```
$ git config --global color.ui true
```

減少輸入字數，如把 git status 變成 git st
```
$ git config --global alias.st status
$ git config --global alias.ck checkout
$ git config --global alias.rst reset HEAD
$ git config --global alias.log "log --online --graph" 
```

查看Git Configuration內容
```
$ git config --list
```

修改Git Configuration相關設定
```
$ vim ~/.gitconfig
```


## 5.1 新增,初始 Repository
```
$ cd ~            # 回到根目錄
$ mkdir sample    # 創建新目錄sample
$ cd sample       # 切換至sample目錄
$ git init        # 讓Git對這個目錄開始進行版控
$ ls -a           # 顯示路徑下的 git 的隱藏目錄
                  # 已初始化空的 Git 版本庫於 /home/wayne/sample/.git/
                  # 只要.git目錄還在，任何檔案都可以救回來
$ git status      # 查看project狀況
```
## 5.2 把檔案交給 Git 控管

寫README相關資訊
> $ vim README.md

把README.md提交到這個專案的**暫存區（Staging Area**）
> $ git add README.md

:::info
一次加入多個 c語言檔案到暫存區
> $ git add *.c

一次加入當前目錄下的全部檔案到暫存區
> $ git add --all
:::

:::warning
若 git add 檔案後，又再進行修改，需要再 git add 一次，才會對再更改的檔案做紀錄。
:::

查看當前目錄狀態
> $ git status

把**暫存區**的東西放到**儲存庫（Repository）**
> $ git commit -m "init commit"
> -m "init commit" 是指要說明 **"你在這次 Commit 做了什麼事情"**，一定要寫清楚。

![](https://i.imgur.com/yZyKev5.png)

## 5.3 工作區、暫存區、儲存庫
二段式管控，先 git add，再 git commit
```flow
st=>start: 工作目錄 Working Directory
e=>end: 儲存庫 Repository
op=>operation: 暫存區域 Staging Area
st->op->e
```

什麼時候要 Commit??
1. 完成一個專案的一個小任務
2. 下班要回家前
3. 自己爽就好


## 5.4 檢視紀錄
查看當前目錄的 Git 紀錄
> $ git log

查看當前目錄的 Git 紀錄 (更精簡)
> $ git log --oneline --graph

找尋某個人的 Commit
> $ git log --author="Zhiwei022"

找尋多個人的 Commit
> $ git log --author="Zhiwei022\|Eddie"

找 Commit 訊息裡有在罵髒話的
> $ git log --grep="wtf"

找哪些 Commit 裡有提到"Ruby"這個字？
> $ git log -S "Ruby"

查看某段時間內的 Commit，如早上9-12
> $ git log --since="9am" --until="12am"

查看某個日期後，某段時間內的 Commit，如2023-01後的早上9-12
> $ git log --since="9am" --until="12am" --after="2023-01"


## 5.5 （狀況題）在 Git 裡刪除檔案或變更檔名
### 刪除檔案
#### 直接刪
使用系統命令 rm 直接刪
> $ rm README.md

把刪除檔案這個**修改動作**加到暫存區
> $ git add README.md

#### 請 Git 幫你砍
使用 Git 命令 rm 直接刪
> $ git rm README.txt

不想刪除檔案，只想取消 Git 對該檔案的管控，而會變成 Untracked files
> $ git rm README.txt --cached

:::info
若使用系統命令刪檔案，則需要二段式動作
若使用 Git命令刪檔案，則會自動完成把刪除檔案這個**修改動作**加到暫存區這個動作。
:::

### 變更檔名
#### 直接變更
使用系統命令 mv 直接更改
> $ mv README.md TEACH.md

用 git status，發現README.md被刪除了，TEACH.md為 Untracked files
> $ git status

把變更檔名這個**修改動作**加到暫存區
> $ git add --all

#### 請 Git 幫你變更
使用 Git 命令 rm 直接變更
> $ git mv README.md TEACH.md

:::info
若使用系統命令變更檔名，則需要二段式動作
若使用 Git命令變更檔名，則會自動完成把變更檔名這個**修改動作**加到暫存區這個動作。
:::

## 5.6 （狀況題）修改Commit紀錄
4種方法：
1. 把.git目錄直接刪掉（誤）
2. 使用 git rebase 來修改歷史
3. 先把 Commit 用 git reset 拆掉，整理後在重新 Commit
4. 使用 --amend 參數來修改最後一次的Commit

### 使用 --amend 參數來修改最後一次的 Commit
> $ git commit --amend -m "modify to new commit"

:::info
若要修改更早之前的 Commit，要使用 git rebase。
:::

## 5.7（狀況題）追加檔案到最近一次的 Commit
1. 使用 git reset 把最後一次 Commit 拆掉，加入新檔案後再 Commit。
2. 使用 --amend 參數進行 Commit。
```linux
$ git add README.md
$ git commit --amend --no-edit
# --no-edit 是指不要編輯 Commit 訊息
```

## 5.8（狀況題）新增目錄？
:::warning
空的目錄無法被加入到 Git 裡面
:::

```linux
$ mkdir images              # 創建新的資料夾
$ touch images/.keep        # 讓 Git 能感應資料夾
$ touch images/.gitkeep     # 讓 Git 能感應資料夾
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	images/

nothing added to commit but untracked files present (use "git add" to track)

$ git add images
$ git commit -m "add new folder images"
```

## 5.9（狀況題）建立.gitignore放入不想進入Git的東西
比較**機密**的檔案，或是**編譯檔或是暫存檔**，就不需要放入 Git 裡面。
.gitignore加入後，只要有對應的檔案加入，都會被 Git **無視**。
```linux
$ touch .gitignore
$ vim .gitignore
$ git add .gitignore
$ git commit -m "add .gitignore"
```

:::info
.gitignore 是對.gitignore 出現後的檔案才會生效，在.gitignore出現前就有的檔案則不適用，除非 $ git rm --cached 後才會生效。
:::

一口氣清除被忽略的檔案，使用 git clean 搭配 -X參數
```linux
$ git clean -fX
# -f 參數是指強制刪除的意思
```

## 5.10（狀況題）檢視特定檔案的 Commit 紀錄
```linux
$ git log test.txt 
commit 8ff5feffc902409d82e7d40b3332614d758e0a53
Author: Zhiwei022 <wayne64081143@gmail.com>
Date:   Mon Mar 20 00:25:05 2023 +0800

    update test.txt

commit e2af5727dd8d3b2c0415fa3273aff49c09d7022d
Author: Zhiwei022 <wayne64081143@gmail.com>
Date:   Mon Mar 20 00:24:39 2023 +0800

    update test.txt
```

```linux
# -p 為看到更詳細的更改地方
$ git log -p test.txt 
commit 8ff5feffc902409d82e7d40b3332614d758e0a53
Author: Zhiwei022 <wayne64081143@gmail.com>
Date:   Mon Mar 20 00:25:05 2023 +0800

    update test.txt

diff --git a/test.txt b/test.txt
index a7f4213..7b9705b 100644
--- a/test.txt
+++ b/test.txt
@@ -1,3 +1,5 @@
 first add the test.txt
 
 update fitst time
+
+update second time

```

:::info
前面加號＋是表示新增，而減號-表示刪除
:::

## 5.11（狀況題）查看這行程式碼是誰 Commit 寫的
```git
# 可以看出那一行是誰寫的
$ git blame hello.c
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800  1) #include <stdio.h>
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800  2) int main(int argc, char *argv[])
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800  3) {
cb29e1fa (Zhiwei022 2023-03-20 00:35:03 +0800  4)     int age = 21;
e95f32f9 (Zhiwei022 2023-03-20 00:42:53 +0800  5)     int year = 0;
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800  6)     printf("%s\n", "Hello World!");
cb29e1fa (Zhiwei022 2023-03-20 00:35:03 +0800  7)     printf("My age is %d\n", age);
e95f32f9 (Zhiwei022 2023-03-20 00:42:53 +0800  8)     printf("I am gradute in NCKU CSIE!!\n");
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800  9)     return 0;
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800 10) }
cb29e1fa (Zhiwei022 2023-03-20 00:35:03 +0800 11) 
```

```git
# 加上 -L 參數，顯示指定行數的內容
$ git blame -L 4,9 hello.c
cb29e1fa (Zhiwei022 2023-03-20 00:35:03 +0800 4)     int age = 21;
e95f32f9 (Zhiwei022 2023-03-20 00:42:53 +0800 5)     int year = 0;
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800 6)     printf("%s\n", "Hello World!");
cb29e1fa (Zhiwei022 2023-03-20 00:35:03 +0800 7)     printf("My age is %d\n", age);
e95f32f9 (Zhiwei022 2023-03-20 00:42:53 +0800 8)     printf("I am gradute in NCKU CSIE!!\n");
4a0045b6 (Zhiwei022 2023-03-20 00:32:43 +0800 9)     return 0;
```

## 5.12（狀況題）還原不小心刪除的檔案或目錄
情境一：不小心刪除了
```git
$ rm README.md

$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    README.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git checkout README.md
$ git status
On branch master
nothing to commit, working tree clean
```

情境二：在未 git add 前， git commit 前，後悔修改檔案，想要回到未修改前的檔案（有Commit過的）
```git
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git checkout test.txt
Updated 1 path from the index

$ git status
On branch master
nothing to commit, working tree clean
```

情境三：已經 git add 後， git commit 前，後悔修改檔案，想要回到未修改前的檔案（有Commit過的）
```git
$ git add test.txt

$ git reset HEAD
Unstaged changes after reset:
M	test.txt

$ git checkout test.txt
Updated 1 path from the index

$ git st
On branch master
nothing to commit, working tree clean
```

## 5.13（狀況題）後悔剛剛的 Commit，想要拆掉重做
```git
$ git log --oneline
e2710a9 (HEAD -> master) update test.txt
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

# 回到指定的 commit
$ git reset 29912da

# 回到e2710a9的前一次
$ git reset e2710a9^

# 回到e2710a9的前二次
$ git reset e2710a9^^

# 回到e2710a9的前7次
$ git reset e2710a9~7

# 回到當前master所在的前一次
$ git reset master^

# 回到當前head所在的前一次
$ git reset HEAD^

```

| 模式                | mixed模式（預設）         | soft模式                    | hard模式                                 |
| ------------------- | ------------------------- | --------------------------- | ---------------------------------------- |
| 工作目錄            | 不變                      | 不變                        | 丟掉                                     |
| 暫存區              | 丟掉                      | 不變                        | 丟掉                                     |
| Commit 拆出來的檔案 | 丟回工作目錄              | 丟回暫存區                  | 直接丟掉                                 |
| git status 狀態     | 尚未git add(內容是改過的) | 已經git add（內容是改過的） | 直接還原目的commit的內容(內容是沒改過的) |



情境四：已經 git commit 後，後悔 Commit，想要回到未修改前的檔案，即前一個commit的內容
```git
$ git reset HEAD^ --hard

$ git status
On branch master
nothing to commit, working tree clean

```


## 5.14（狀況題）不小心使用 hard 模式 Reset 了某個 Commit，還是可以救回來
```git
$ git log --oneline
e2710a9 (HEAD -> master) update test.txt
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

$ git reset HEAD~2
b5db030 (HEAD -> master) add .gitignore
76c1cc0 add new folder images
e95f32f update hello.c
cb29e1f update hello.c
4a0045b add hello.c
8ff5fef update test.txt
e2af572 update test.txt
668d1de add test.txt
d6bc128 init commit

# 還原到最初的檔案內容
$ git reset e2710a9 --hard

$ git log --oneline
e2710a9 (HEAD -> master) update test.txt
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
```