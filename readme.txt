git命令的学习：
安装：sudo apt-get install git
全局配置：
git config --global use.name "Tom Hills"
git config --global use.email "cugzhangtao@126.com"

创建版本库：
新建文件夹或是在已经有文件的文件夹下 git init
在该目录下会存在.git文件夹

提交修改
step1. git add *.*（把需要提交的文件都add）
step2. git commit -m "------" （引号内为此次提交的说明）

虽然设置了email和name，但是在提交的时候还是会提示

即使重新咋设置，还是没有用，解决办法是在.git路径下，编辑config文件，添加emial和name


git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
用git log命令查看历史记录，加上加上--pretty=oneline参数，可以单行显示

git reset --hard HEAD^，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
同样，也可以使用此命令返回到之前最后一个版本，--hard的参数设置为对应的commit ID即可
查看commit id的命令为git reflog
每次修改，如果不add到暂存区，那就不会加入到commit中。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

Git鼓励大量使用分支：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用git log --graph命令可以看到分支合并图。

在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场

开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

因此，多人协作的工作模式通常是这样：
  1. 首先，可以试图用git push origin branch-name推送自己的修改；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。
这就是多人协作的工作模式，一旦熟悉了，就非常简单。

  ● 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
  ● git tag -a <tagname> -m "blablabla..."可以指定标签信息；
  ● git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
  ● 命令git tag可以查看所有标签。

  ● 命令git push origin <tagname>可以推送一个本地标签；
  ● 命令git push origin --tags可以推送全部未推送过的本地标签；
  ● 命令git tag -d <tagname>可以删除一个本地标签；
  ● 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

  ● 在GitHub上，可以任意Fork开源仓库；
  ● 自己拥有Fork后的仓库的读写权限；
  ● 可以推送pull request给官方仓库来贡献代码。

练习：github上仓库地址为git@github.com:yinjiwenhou/learn-git.git，在本地新建文件夹，然后下载远程仓库的代码
在使用Github和本地之间的传输是加密的，所以需要设置在github上添加公钥，用来标识提交人
新建文件夹： mkdir git
同步仓库：git clone git@github.com:yinjiwenhou/learn-git.git

