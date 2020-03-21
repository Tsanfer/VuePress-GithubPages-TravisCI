## 创建Github仓库

在Github官网上创建一个新的仓库（我仓库的名字叫`VuePress-GithubPages-TravisCI`）

### SSH密钥链接Github

#### 生成SSH密钥

```shell
$ ssh-keygen
Generating public/private rsa key pair.
# 输入准备存放密钥的位置，公钥和私钥放在同一个文件夹
Enter file in which to save the key (/home/tsanfer/.ssh/id_rsa): /home/tsanfer/.ssh/test_key
# 输入口令，不需要口令就直接回车，这里我不需要口令，直接回车
Enter passphrase (empty for no passphrase):
# 确认口令，跟上面一样
Enter same passphrase again:
# 显示私钥位置
Your identification has been saved in /home/tsanfer/.ssh/test_key.
# 显示公钥位置，下一步需要添加公钥到Github中
Your public key has been saved in /home/tsanfer/.ssh/test_key.pub.
```

#### Github添加SSH密钥

在Github头像旁边的菜单中 Settings --> SSH and GPG keys --> SSH keys 中的右上角点击 New SSH key



下面要填入信息：

- Title：随便填
- Key：公钥文件里的所有内容（`~/.ssh/test_key.pub`）

#### 测试SSH密钥

测试一下密钥

```shell
ssh -T git@github.com
```

设置Github账号的地址

```shell
git config --global user.name "username"
git config --global user.email "useremail"

# 比如
git config --global user.name "Tsanfer"
git config --global user.email "a1124851454@gmail.com"
```

如果成功的话

```shell
Hi Tsanfer! You've successfully authenticated, but GitHub does not provide shell access.
```

用SSH的方式克隆仓库到本地

```shell
# 选一个文件夹克隆仓库
# 比如家目录
cd ~
git clone git@github.com:{Username}/{Repo}.git

# 比如
# cd ~
# git clone git@github.com:Tsanfer/VuePress-GithubPages-TravisCI.git
```

```shell
# 克隆完之后的目录
~/VuePress-GithubPages-TravisCI/
```

