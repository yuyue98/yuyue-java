[【返回首页】](../README.md) | [【返回上一页】](../README.md)

---

<h1 align="center" style="text-align:center;vertical-align:middle;">
  Git学习笔记
</h1>

## Git学习笔记

### Git命令

#### Git基础命令

* 查看修改内容
  * `git diff --cached`
* 内容提交
  * `git commit -m "提交信息"`

#### Git子模块命令

* 添加子模块
  * `git submodule add <url> <path>`
  * 查看信息可以看到增加了子模块 `git diff --cached`
  * 提交即完成子模块的添加 `git commit -m "add submodule"`
* 克隆含有子模块的项目
    * 初始化本地配置文件 `git submodule init`
    * 从项目中抓取所有数据并检出父项目中列出的合适的提交 `git submodule update`
    * 初始化并更新子模块 `git submodule update --init`
    * 初始化、抓取并检出任何嵌套的子模块
        * `git submodule update --init --recursive`
        * `git clone --recurse-submodules`
    * 更新子模块
        * [v1.8.2+] `git submodule update --recursive --remote`
        * [v1.7.3+] `git submodule pull --recursive-submodules`
* 更新子模块
  * 子模块的维护者提交了更新后，使用子模块的项目必须手动更新才能包含最新的提交。 
  * 在项目中，进入到子模块目录下，执行`git pull`更新，查看`git log`查看相应提交。 
  * 完成后返回到项目目录，可以看到子模块有待提交的更新，使用`git add`，提交即可。
* 删除子模块
  * `rm -rf` 子模块目录 删除子模块目录及源码 
  * `vi .gitmodules` 删除项目目录下.gitmodules文件中子模块相关条目，并提交.gitmodules文件变更
  * `vi .git/config` 删除配置项中子模块相关条目 
  * `rm .git/module/*` 删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
  * 执行完成后，再执行添加子模块命令即可，如果仍然报错，需要执行 `git rm --cached 子模块名称`
  * 完成删除后，将修改提交到仓库

### Git注意点

#### 1 git submodule 的坑

##### 1.1 更新submodule的坑

【问题描述】

* 他人更新submodule且更新父项目中依赖版本号的情况
* 这种情况下，直接git pull之后，忘记调用git submodule update
* 极有可能再次把旧的submodule依赖信息提交上去

【问题解决】

* git pull之后，立即执行 git status
* 如果发现submodule有修改，立即执行 git submodule update
* 尽量不要使用git commit -a
* git add存在的意义就是对加入暂存区的文件做二次确认
* git commit -a 相当于跳过了确认过程
* 子模块嵌套的情况，使用命令 git submodule foreach git submodule update

##### 1.2 修改submodule的坑

【问题描述】

* 对submodule做修改时，往往是切到wubmodule的目录，修改后做commit和push
* 默认git submodule update 不会讲submodule 切到任何branch
* 默认情况下，submodule的HEAD是处于游离状态的
* 修改前，需要用git checkout master 将当前的分支切换到master之后做修改

【问题解决】

* 不慎忘记切换master分支又做了提交的挽救方法
* 用`git checkout master`将HEAD从游离状态切换到master分支
* 记住git警告中这次提交的change-id
* 用`git cherry-pick <change-id>`将之前的提交作用在master分支上
* 用`git push`将更新提交到远程版本库中
