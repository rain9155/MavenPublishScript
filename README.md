# MavenPublishScript
### gradle发布组件到MavenCentral脚本，支持aar和jar发布

## 使用方法
首先在你的组件的build.gradle中apply该脚本：
```groovy
apply from: 'https://raw.githubusercontent.com/rain9155/MavenPublishScript/main/script/publication.gradle'
```
然后在组件的gradle.properties(没有则创建)中添加组件信息，其中GAV坐标是必填信息，其他是可选信息：
```
# GAV坐标
GROUPID=io.github.rain9155
ARTIFACTID=baseadapter
# 版本加SNAPSHOT后缀可发布到maven远程snapshot地址，如1.0.3-SNAPSHOT
VERSION=1.0.3

# 可选信息
DESCRIPTION=封装RecyclerView的Adapter，减少Adapter重复代码的编写，支持多种类型的itemType、自动加载更多、添加emptyView和添加headerView
URL=https://github.com/rain9155/BaseAdapter
LICENSENAME=The Apache License, Version 2.0
LICENSEURL=http://www.apache.org/licenses/LICENSE-2.0.txt
DEVELOPERNAME=rain9155
DEVELOPEREMAIL=jianyu9155@gmail.com
# scm信息，格式参考http://maven.apache.org/scm/scm-url-format.html
SCMURL=https://github.com/rain9155/BaseAdapter/tree/master
SCMCONNECTION=scm:git:git://github.com/rain9155/BaseAdapter.git
SCMDEVELOPERCONNECTION=scm:git:ssh://github.com:rain9155/BaseAdapter.git
# 如果发布的是android组件，为true时，支持根据flavor动态生成组件名称，规则为ARTIFACTID-{flavorName}，默认为false
APPENDFLAVORNAME=true
```
然后在项目根目录的local.properties(没有则创建)中添加gpg签名信息和ossrh账号信息，记得要把local.properties从你的版本控制中移除，避免泄漏你的签名信息和账号信息：
```
# gpg签名信息
signing.keyId=你的密钥id
signing.password=你的私钥密码
signing.secretKeyRingFile=你的私钥文件路径

# ossrh账号信息
ossrh.username=你的ossrh账号名
ossrh.password=你的ossrh账号密码
```
最后在命令行执行gradle任务发布组件，如果你发布的是android组件(aar)，执行的任务的名称格式为`publish{flavorName}LibraryPublicationToMavenRepository`或`publish{flavorName}LibraryPublicationToMavenLocal`，其中flavorName为android组件的flavor的名称，首字母大写，没有则不填，如果你发布的是java组件(jar)，执行的任务名称为`publishJarPublicationToMavenRepository`或`publishJarPublicationToMavenLocal`：
```bash
//发布aar到maven本地仓库
gradle publishLibraryPublicationToMavenLocal

//发布aar到maven远程release或snapshot仓库
gradle publishLibraryPublicationToMavenRepository

//假设android组件含有flavorName为china，发布china版本的aar到maven本地仓库
gradle publishChinaLibraryPublicationToMavenLocal

//假设android组件含有flavorName为oversea， 发布oversea版本的aar到maven远程release或snapshot仓库
gradle publishOverseaLibraryPublicationToMavenRepository

//发布jar到maven本地仓库
gradle publishJarPublicationToMavenLocal

//发布jar到maven远程release或snapshot仓库
gradle publishJarPublicationToMavenRepository
```
更详细使用可以查看[demo](https://github.com/rain9155/MavenPublishScript/tree/main/demo)
