# MavenPublishScript
### 发布组件到Maven仓库的gradle脚本，支持aar和jar发布, 如有问题欢迎[issue](https://github.com/rain9155/MavenPublishScript/issues)
## 使用方法
首先在你的组件的build.gradle中apply该脚本：
```groovy
apply from: 'https://raw.githubusercontent.com/rain9155/MavenPublishScript/main/script/publication.gradle'
```
有时候github地址在国内会访问失败，可以apply码云地址的脚本：
```groovy
apply from: 'https://gitee.com/rain_9155/hosting/raw/master/MavenPublishScript/publication.gradle'
```
然后在组件的gradle.properties(没有则创建)中添加组件信息，其中GAV坐标是必填信息，其他是可选信息：
```groovy
### GAV坐标
publish.groupId=io.github.rain9155
# 如果是android组件并且有flavor，最终生成的artifactId会拼接flavorName信息，拼接规则为artifactId-{flavorName}，可以设置isAppendFavorName为false取消拼接
publish.artifactId=mavenpublishscript
# 版本加SNAPSHOT后缀可发布到maven远程snapshot地址，如1.0.0-SNAPSHOT，如果没有SNAPSHOT后缀则默认发布到maven远程release地址
publish.version=1.0.0

### 下面都是可选信息
# 支持更换发布的仓库地址(release或snapshot)，如果你想发布到私有仓库、本地目录等，可以在这里设置，默认发布到Sonatype OSSRH
publish.repoReleaseUrl=./repo/release
publish.repoSnapshotUrl=./repo/snapshot

# 支持二次打包本地aar或jar组件发布到仓库，只要在这里填写组件的本地地址就行，多个组件地址用英文分号;隔开，发布时使用对应的任务
# 二次打包生成的artifactId不使用publish.artifactId指定的，而是使用artifactPath传入的文件名，如这里为lib1、lib2、lib3
publish.artifactPath=./libs/lib1.aar;./libs/lib2.aar;./libs/lib3.jar

# 如果发布的是android组件，当为false时不根据flavor动态生成组件的artifactId，如果你不想组件的artifactId拼接flavorName，可以设置为false，默认为true
publish.artifactId.isAppendFavorName=false

# 支持跳过签名校验，当为true时不进行签名，如果你发布的组件不想进行gpg签名，可以设置为true，默认为false
publish.isSkipSignature=true

# 支持跳过账号校验，当为true时不进行账号信息校验，如果你的仓库地址不需要账号就能访问，可以设置为true，默认为false
publish.isSkipCredential=true

# 基本描述
publish.description=发布组件到Maven仓库的gradle脚本，支持aar和jar发布
publish.url=https://github.com/rain9155/MavenPublishScript

# 开发者信息
publish.developerName=rain9155
publish.developerEmail=jianyu9155@gmail.com

# license信息
publish.licenseName=The Apache License, Version 2.0
publish.licenseUrl=http://www.apache.org/licenses/LICENSE-2.0.txt

# scm信息，格式参考http://maven.apache.org/scm/scm-url-format.html
publish.scmUrl=https://github.com/rain9155/MavenPublishScript/tree/master
publish.scmConnection=scm:git:git://github.com/rain9155/MavenPublishScript.git
publish.scmDeveloperConnection=scm:git:ssh://github.com:rain9155/MavenPublishScript.git
```
然后在项目根目录的local.properties(没有则创建)中添加gpg签名信息和ossrh账号信息，记得要把local.properties从你的版本控制中移除，避免泄漏你的签名信息和账号信息：
```
# gpg签名信息，如果设置了isSkipSignature=true跳过签名，则可以不填
signing.keyId=你的密钥id
signing.password=你的私钥密码
signing.secretKeyRingFile=你的私钥文件路径

# ossrh账号信息，如果设置了isSkipCredential=true跳过账号校验，则可以不填
ossrh.username=你账号的用户名
ossrh.password=你账号的密码
```
最后在命令行执行gradle任务发布组件，如果你发布的是android组件(aar)，执行的任务的名称格式为`publish{flavorName}AndroidlibPublicationToMavenRepository`或`publish{flavorName}AndroidlibPublicationToMavenLocal`，其中flavorName为android组件的flavor的名称，首字母大写，没有则不填flavorName，如果你发布的是java组件(jar)，执行的任务名称为`publishJavalibPublicationToMavenRepository`或`publishJavalibPublicationToMavenLocal`，如果你想二次打包artifactPath指定的aar或jar组件发布，执行的任务名称为`publishPack{fileName}libPublicationToMavenRepository`或`publishPack{fileName}libPublicationToMavenLocal`，其中fileName为artifactPath传入的组件路径中的文件名，首字母大写,下面为gradle发布任务执行示例：
```bash
//发布android组件到maven本地仓库
gradle publishAndroidlibPublicationToMavenLocal

//发布android组件到maven远程release或snapshot仓库
gradle publishAndroidlibPublicationToMavenRepository

//假设android组件含有flavorName为china，发布china版本的android组件到maven本地仓库
gradle publishChinaAndroidlibPublicationToMavenLocal

//假设android组件含有flavorName为oversea， 发布oversea版本的android组件到maven远程release或snapshot仓库
gradle publishOverseaAndroidlibPublicationToMavenRepository

//发布java组件到maven本地仓库
gradle publishJavalibPublicationToMavenLocal

//发布java组件到maven远程release或snapshot仓库
gradle publishJavalibPublicationToMavenRepository

//假设artifactPath指定了lib1.aar、lib2.aar、lib3.jar路径，发布artifactPath指定的aar或到maven本地仓库
gradle publishPackLib1libPublicationToMavenLocal publishPackLib2libPublicationToMavenLocal publishPackLib3libPublicationToMavenLocal

//假设artifactPath指定了lib1.aar、lib2.aar、lib3.jar路径，发布artifactPath指定的aar或jar到maven远程release或snapshot仓库
gradle publishPackLib1libPublicationToMavenRepository publishPackLib2libPublicationToMavenRepository publishPackLib3libPublicationToMavenRepository

//发布所有android组件、java组件和指定的aar或jar到maven本地仓库
gradle publishToMavenLocal

//发布所有android组件、java组件和指定的aar或jar到maven远程release或snapshot仓库
gradle publishAllPublicationsToMavenRepository

//发布所有android组件、java组件和指定的aar或jar到maven本地仓库和maven远程release或snapshot仓库
gradle publish
```
更详细使用可以查看[demo](https://github.com/rain9155/MavenPublishScript/tree/main/demo)
