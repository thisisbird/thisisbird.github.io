###### tags: `教學` `git`

# Git 指令

1. 初始化專案
    ```
    git init
    ```
1. 加入檔案
    ```
    git add .
    git add *.txt
    ```
1. 提交
    ```
    git commit
    git commit -m "First Commit"
    git push
    ```
1. 其他指令
    >git checkout -b {branch_name} origin/master
    >
    >在本機端，建立一個追縱 origin/master 的 {branch_name}，並切換目前的 branch    到 {branch_name}

    ```
    git checkout
    git checkout master
    git pull
    git status
    ```
1. test
    ```
    git reset --hard origin/cosmosdesign
    ```