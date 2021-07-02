# MavenPublishScript
### gradle发布组件到MavenCentral脚本，支持aar和jar发布
## 使用方法
首先在你的组件的build.gradle中apply该脚本：
```groovy
apply from: 'https://raw.githubusercontent.com/rain9155/MavenPublishScript/main/script/publication.gradle'
```
然后在gradle.properties中添加组件信息，其中GAV坐标是必填信息，其他是可选信息：
```groovy
# GAV坐标
GROUPID=io.github.rain9155
ARTIFACTID=baseadapter
# 版本加SNAPSHOT后缀可发布到maven远程snapshot地址，如1.0.3-SNAPSHOT
VERSION=1.0.3

#可选信息
DESCRIPTION=封装RecyclerView的Adapter，减少Adapter重复代码的编写，支持多种类型的itemType、自动加载更多、添加emptyView和添加headerView
URL=https://github.com/rain9155/BaseAdapter
LICENSENAME=The Apache License, Version 2.0
LICENSEURL=http://www.apache.org/licenses/LICENSE-2.0.txt
DEVELOPERNAME=rain9155
DEVELOPEREMAIL=jianyu9155@gmail.com
SCMURL=https://github.com/rain9155/BaseAdapter.git
SCMCONNECTION=git:git://github.com/rain9155/BaseAdapter.git
SCMDEVELOPERCONNECTION=git:ssh://github.com/rain9155/BaseAdapter.git
```

然后在local.properties中添加gpg签名信息和ossrh账号信息，记得要把local.properties从你的版本控制中移除，避免泄漏你的签名信息和账号信息：
```groovy
#gpg签名信息
signing.keyId=你的密钥id
signing.password=你的私钥密码
signing.secretKeyRingFile=你的私钥文件路径

#ossrh账号信息
ossrh.username=你的ossrh账号名
ossrh.password=你的ossrh账号密码
```
