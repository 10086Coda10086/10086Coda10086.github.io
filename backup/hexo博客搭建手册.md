## 注：教程以Windows系统为例

## 1. 安装[Node.js](https://nodejs.org/dist/v22.15.0/node-v22.15.0-x64.msi)
## 2. 安装[git](https://git-scm.com/downloads/win)
**一直点确定就行，全部按它默认勾的，若要细细配置可参照[这篇文章](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)**
## 3. 测试一下，管理员运行cmd，依次输入
~~~
node -v
npm -v
git -v
~~~
若正常显示版本号即可
## 4. 下载hexo
1. **新建一个文件夹**，用来存放blog
2. 右键，选择**Open git bash here**
3. 依次输入
~~~
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
~~~
## 5. 简单认识hexo
1. 进入自动创建的blog文件夹
2. **_config.yml**，在这个文件内修改博客基础设置
3. **blog\source\_posts**，在这个文件夹内修改博客文章
4. **themes**，在这个文件夹内修改主题
5. 在自动创建的blog文件夹内右键，选择**Open git bash here**
6. 基础指令：
~~~	
# 开启服务
hexo s

# 新建文章
hexo new 文章名

# 新建草稿
hexo new draft 文章名

# 发布草稿成为文章
hexo publish 文章名

# 普通页面
hexo new page 页面名

# 清空缓存
hexo cl

# 生成静态页面
hexo g
# 然后访问localhost:4000

# 部署文章
hexo d
~~~
## 6. 搭建仓库
1. 注册/登入[Github](https://github.com/)（可能无法访问，建议使用[watt tolkit](https://steampp.net/)）
2. 点击**Create a new repository**进入新建仓库页面，仓库名输入：
~~~
你的GitHub用户名.github.io
~~~
3. 勾选 **Public**
4. 勾选 **Add a README file**
5. 拉到下面点击create创建
## 7. 生成SSH Keys
1. 进入任意文件夹，右键空白处然后点**Open Git bash here**,输入
~~~
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
~~~
2. 敲4次Enter
3. 进入**C:\Users\用户名**，在里面进入 **.ssh文件夹**
4. 打开里面的**id_rsa.pub**，全选复制里面的代码
5. 打开github
6. 进入**用户设置**，找到**SSH keys**
7. **新建SSH keys**，名称随意，在下面**粘贴代码**，然后**创建**
## 8. 测试是否成功
1. 在git bash中输入
~~~
ssh -T git@github.com
~~~
回车，然后再输入**yes**
## 9. 配置_config.yml
1. 进入之前的**Blog**文件夹，打开 **_config.yml**
2. 拉到最下面将**deploy**后面的全删掉，复制粘贴这段
~~~
  type: git
  repository: 
  branch: main
~~~
{% note success %}
注意缩进格式：每行前面都有两个空格不要删，每个冒号后面都有个空格也不要删！
{% endnote %}

3. 去github之前生成的仓库页面，点**code**，**复制https链接**，将其粘贴到**repository：**后面，然后保存退出
# 10. 安装自动部署发布工具
1. 回到博客文件夹，一样git bash
2. 输入：
~~~
npm install hexo-deployer-git --save
~~~
3. 在Blog文件夹右键打开git bash，依次输入
~~~
hexo g
hexo d
~~~
如果是第一次使用git的话会需要配置
~~~
git config --global user.email
~~~
user.email即你注册GitHub的邮箱
~~~
git config --global user.name
~~~
user.name即你注册GitHub的名字

4. 配置完后再输入
~~~
hexo d
~~~
，然后在跳出来的窗口内进行登录
接下来我们就成功把本地内容上传到github了
## 11.上传成功
上传成功以后，我们就算搭建好了！上自己的网址看看吧
网址是我们之前设的仓库名：**你的GitHub用户名.github.io**
