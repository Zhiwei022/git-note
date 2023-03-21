# Git教學 Chapter8 標籤

:::info
本文章是依據 "為你自己學Git--高見龍著" 這本書的內容所寫的。
:::

###### tags: `Git教學`
[TOC]

## 8.1 使用標籤

### 標籤是什麼
跟分支一樣，都是指向某一個 Commit，但實際不一樣。

### 什麼時候使用標籤
完成開發的特定里程碑時，例如軟體版本1.0.0，或是 beta-releasa。

### 標籤有2種
1. 輕量標籤(lightweight tag)
    > 只有標籤名，個人使用或暫時標記用途。
3. 附註標籤(annotated tag)
    > 有更多關於這張 tag 的訊息。

### 輕量標籤(lightweight tag)
![](https://i.imgur.com/WODDN6m.png)

**1. 用指令**
```
# 在 7e9de43 commit 新增 tag all_dogs
$ git tag all_dogs 7e9de43

# 查看 tag 狀況
$ git log --oneline
7e9de43 (HEAD -> master, tag: all_dogs) add dog3.c
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
```

**2. 用sourcetree (方便)**
直接在對 Commit  右鍵，選 Tag..，勾選Lightweight tag(not recommended)，不用寫 message。
![](https://i.imgur.com/dCNb9iq.png)



### 附註標籤(annotated tag)
**1. 用指令**
```
# 在 7e9de43 commit 新增 tag all_dogs
$ git tag all_dogs 7e9de43

# 查看 tag 狀況
$ git log --oneline
7e9de43 (HEAD -> master, tag: all_dogs) add dog3.c
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
```

**2. 用sourcetree (方便)**
直接在對 Commit  右鍵，選 Tag..，不要勾Lightweight tag(not recommended)，但要寫 message。
![](https://i.imgur.com/dSdLfc5.png)


### 查看標籤
```git
$ git show tag_name
```

### 刪除標籤
```git
$ git tag -d tag_name
```

## 8.2 (冷知識)標籤跟分之有什麼不一樣
### 內容
內容都是放 40 個字元的 SHA-1 值。

### 位置
分支在 .git/refs/heads 目錄下
標籤在 .git/refs/tags 目錄下

### 移動
分支會隨著 Commit 移動而移動。
標籤不會移動，一旦貼上去，就固定在那個 Commit 上了。


