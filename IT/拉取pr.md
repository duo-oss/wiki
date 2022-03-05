pull pr
当别人提交了代码  你需要拉取下来 看的时候 可以定向的 拉取 别人的pr

git fetch 仓库地址 pull/pr号/head:分支名

	git fetch https://gitee.com/re_tech/datart.git pull/17/head:pr_17


上面的操作就是 拉取更新 这个仓库的 17号 pr 拉取到本地 并创建了一个新的分支 命名为 pr_17

如果是 自己仓库的上游分支 可以 用 upstream 代替仓库地址

	git fetch upstream pull/17/head:pr_17 


其中 pr_17 是自己定义的 叫 ssqqw 也是可以的


	git fetch reDatart pull/258/head:pr_258


#git


