---
title: "Sync Obsidian with Github"
author: adrian
type: post
url: /2023/02/snyc-obsidian-with-github/
categories:
  - Note
  - Obsidian
  - Github
tags:
  - note
  - github
  - obsidian
date: 2023-02-25T21:14:28+08:00
---

先備知識
- [github](https://docs.github.com/en/get-started/quickstart/hello-world)
- [obsidian](https://obsidian.md/)

## Linux and Mac OS X

![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2023/02/25/obsidian-graph-view-linux.webp)

1. 安裝 git

    Mac OS X (透過 Homebrew)

    ```bash
    brew install git
    ```

    Linux (Debian/Ubuntu)

    ```bash
    sudo apt-get install git
    ```

2. Clone the repository

    進入存放`repository`的`folder`
    
    ```bash
    cd ~/repositories
    ```

    clone
    ```bash
    git clone https://github.com/{your_username}/{your_repository}

    ```

3. 在`Obsidian`開啟`vault`,並選則該`repository`

## Android

![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2023/02/25/obsidian-graph-view.webp)

1. 安裝Termux

    [Google Play](https://play.google.com/store/apps/details?id=com.termux&hl=zh_TW&gl=US)

2. 更新`pkg`資料庫 && 升級已安裝軟體

    ```
    pkg update && pkg upgrade -y
    ```

3. 安裝`git`的client端

    ```
    pkg install git
    ```

4. 給`Termux`訪問儲存空間的權限

    ```
    termux-setup-storage
    ```

5. 進入`shared`目錄

    ```
    cd /storage/shared
    ```

6. 創建`Obsidian`使用的目錄並進入

    ```
    mkdir obsidian
    cd obsidian
    ```

7. Clone the repository

    因為在 `android`手機上打字不那麼方便,我會將`token`存在`~/.git-credentials`.下次`git pull`就不用再輸入一次.
    > 注意 : ~/.git-credentials 會以明文儲存,確保你沒有給你的 token 太大的權限

    ```
    git config --global credential.helper store
    ```

    git clone

    ```
    git clone https://github.com/{your_username}/{your_repository}
    ```

    Push && Pull

    因為已經儲存`github token`了,`pull`跟`push`時不用再輸入一次`username`跟`password`
    
    ```
    git pull
    ```

    Push

    ```
    git push
    ```

8. 在`Obsidian`開啟`vault`

    ![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2023/02/25/open-folder-as-vault.webp)
