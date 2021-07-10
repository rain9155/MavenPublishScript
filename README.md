# MavenPublishScript
### gradle发布组件到MavenCentral脚本，支持android组件和java组件发布，如有问题欢迎[issue](https://github.com/rain9155/MavenPublishScript/issues)

## 使用方法
首先在你的组件的build.gradle中apply该脚本：
```groovy
apply from: 'https://raw.githubusercontent.com/rain9155/MavenPublishScript/main/script/publication.gradle'
```
然后在组件的gradle.properties(没有则创建)中添加组件信息，其中GAV坐标是必填信息，其他是可选信息：
```
# GAV坐标
GROUPID=io.github.rain9155
ARTIFACTID=mavenpublishscript
# 版本加SNAPSHOT后缀可发布到maven远程snapshot地址，如1.0.0-SNAPSHOT
VERSION=1.0.0

# 可选信息
DESCRIPTION=gradle发布组件到MavenCentral脚本，支持android组件和java组件发布
URL=https://github.com/rain9155/MavenPublishScript
LICENSENAME=The Apache License, Version 2.0
LICENSEURL=http://www.apache.org/licenses/LICENSE-2.0.txt
DEVELOPERNAME=rain9155
DEVELOPEREMAIL=jianyu9155@gmail.com
# scm信息，格式参考http://maven.apache.org/scm/scm-url-format.html
SCMURL=https://github.com/rain9155/MavenPublishScript/tree/master
SCMCONNECTION=scm:git:git://github.com/rain9155/MavenPublishScript.git
SCMDEVELOPERCONNECTION=scm:git:ssh://github.com:rain9155/MavenPublishScript.git
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
最后在命令行执行gradle任务发布组件，如果你发布的是android组件，执行的任务的名称格式为`publish{flavorName}AndroidlibPublicationToMavenRepository`或`publish{flavorName}AndroidlibPublicationToMavenLocal`，其中flavorName为android组件的flavor的名称，首字母大写，没有则不填flavorName，如果你发布的是java组件，执行的任务名称为`publishJavalibPublicationToMavenRepository`或`publishJavalibPublicationToMavenLocal`：
```bash
//发布android组件到maven本地仓库
gradle publishAndroidlibPublicationToMavenLocal

//发布android组件到maven远程release或snapshot仓库
gradle publishAndroidlibPublicationToMavenRepository

//假设android组件含有flavorName为china，发布china版本的android组件到maven本地仓库
gradle publishChinaAndroidlibPublicationToMavenLocal
j
//假设android组件含有flavorName为oversea， 发布oversea版本的android组件到maven远程release或snapshot仓库
gradle publishOverseaAndroidlibPublicationToMavenRepository

//发布java组件到maven本地仓库
gradle publishJavalibPublicationToMavenLocal

//发布java组件到maven远程release或snapshot仓库
gradle publishJavalibPublicationToMavenRepository

//发布所有android组件和java组件到maven本地仓库
gradle publishToMavenLocal

//发布所有android组件和java组件到maven远程release或snapshot仓库
gradle publishAllPublicationsToMavenRepository

//发布所有android组件和java组件到maven本地仓库和maven远程release或snapshot仓库
gradle publish
```
更详细使用可以查看[demo](https://github.com/rain9155/MavenPublishScript/tree/main/demo)
