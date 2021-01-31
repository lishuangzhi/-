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
  
  
  
 ## 编译中遇到的问题
 
 internal error: Could not find a supported mac sdk: ["10.10" "10.11" "10.12" "10.13" "10.14"]
 
 修改build/soong/cc/config/x86_darwin_host.go文件
 darwinSupportedSdkVersions = []string{
    "10.10",
    "10.11",
    "10.12",
    "10.13",
    "10.14",
    "11.1", //新添加的
}
错误
============================================
[  0% 3/25933] build out/target/product/generic_x86_64/obj/ETC/sepolicy_tests_i
FAILED: out/target/product/generic_x86_64/obj/ETC/sepolicy_tests_intermediates/sepolicy_tests
/bin/bash -c "(out/host/darwin-x86/bin/sepolicy_tests -l out/host/darwin-x86/lib64/libsepolwrap.dylib 		 -f out/target/product/generic_x86_64/obj/ETC/plat_file_contexts_intermediates/plat_file_contexts  -f out/target/product/generic_x86_64/obj/ETC/vendor_file_contexts_intermediates/vendor_file_contexts  -p out/target/product/generic_x86_64/obj/ETC/sepolicy_intermediates/sepolicy ) && (touch out/target/product/generic_x86_64/obj/ETC/sepolicy_tests_intermediates/sepolicy_tests )"
/bin/bash: line 1: 55737 Segmentation fault: 11  ( out/host/darwin-x86/bin/sepolicy_tests -l out/host/darwin-x86/lib64/libsepolwrap.dylib -f out/target/product/generic_x86_64/obj/ETC/plat_file_contexts_intermediates/plat_file_contexts -f out/target/product/generic_x86_64/obj/ETC/vendor_file_contexts_intermediates/vendor_file_contexts -p out/target/product/generic_x86_64/obj/ETC/sepolicy_intermediates/sepolicy )

修改 /Volumes/A/android_work/system/sepolicy/tests 目录下的 Android.bp文件
cc_library_host_shared {
    name: "libsepolwrap",
    srcs: ["sepol_wrap.cpp"],
    cflags: ["-Wall", "-Werror",],
    export_include_dirs: ["include"],

    // libsepolwrap gets loaded from the system python, which does not have the
    // ASAN runtime. So turn off sanitization for ourself, and  use static
    // libraries, since the shared libraries will use ASAN.
    static_libs: [
        "libbase",
        "libsepol",
    ],
    //stl: "libc++_static",
    sanitize: {
        never: true,
    },
}
将stl: "libc++_static"注释掉

  
