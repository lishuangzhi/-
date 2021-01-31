# - 编译Android 源码
  ## - 下载Android源码
  ### - 安装Repo
  mkdir ~/bin
  
  PATH=~/bin:$PATH
  
  下载
  curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  
  chmod a+x ~/bin/repo
  由于国内网络的原因可以使用国内的源
  curl -sSL  'https://gerrit-googlesource.proxy.ustclug.org/git-repo/+/master/repo?format=TEXT' |base64 -d > ~/bin/repo
  chmod a+x ~/bin/repo
  
  创建源码存放目录
  mkdir WORKING_DIRECTORY
  cd WORKING_DIRECTORY
  设置Git信息
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  
  初始化Repo
  repo init -u https://android.googlesource.com/platform/manifest
  如果因为网络原因，可以改成国内的源
  repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest
  如果提示无法连接到 gerrit.googlesource.com，可以编辑 ~/bin/repo，把 REPO_URL 一行替换成下面的：
  REPO_URL = 'https://gerrit-googlesource.proxy.ustclug.org/git-repo'
  
  指定分支
  repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest -b android-4.0.1_r1
  分支列表：https://source.android.com/source/build-numbers.html#source-code-tags-and-builds
  
  开始下载
  repo sync
  
  
