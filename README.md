# MavenPublishScript
### 发布组件到Maven仓库的gradle脚本，支持aar和jar发布

## 使用方法
首先在你的组件的build.gradle中apply该脚本：
```groovy
apply from: 'https://raw.githubusercontent.com/rain9155/MavenPublishScript/main/script/publication.gradle'
```
然后在组件的gradle.properties(没有则创建)中添加组件信息，其中GAV坐标是必填信息，其他是可选信息：
```
### GAV坐标
publish.groupId=io.github.rain9155
publish.artifactId=mavenpublishscript
# 版本加SNAPSHOT后缀可发布到maven远程snapshot地址，如1.0.0-SNAPSHOT
publish.version=1.0.0

### 下面都是可选信息
# 组件的基本描述
publish.description=发布组件到Maven仓库的gradle脚本，支持aar和jar发布
publish.url=https://github.com/rain9155/MavenPublishScript

# 支持更换发布的仓库地址(release或snapshot)，如果你想发布到私有仓库、本地目录等，可以在这里设置，默认发布到Sonatype OSSRH
publish.repoReleaseUrl=./repo/release
publish.repoSnapshotUrl=./repo/shapshot

# 支持二次打包本地aar或jar组件发布到仓库，只要在这里填写组件的本地地址就行，多个组件地址用英文分号;隔开
publish.artifactPath=./libs/javalib.jar;./libs/androidlib,aar

# 如果发布的是android组件，为true时，支持根据flavor动态生成组件名称，规则为artifactId-{flavorName}，默认为false
publish.isAppendFavorName=true

# 支持跳过签名校验，为true时不进行签名，如果你发布的组件不想进行gpg签名，可以设置为true，默认为false
publish.isSkipSignature=true

# 支持跳过账号校验，为true时不进行账号信息校验，如果你的仓库地址不需要账号就能访问，可以设置为true，默认为false
publish.isSkipCredential=true

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
# gpg签名信息，如果设置了SKIP_SIGNATURE跳过签名，则可以不填
signing.keyId=你的密钥id
signing.password=你的私钥密码
signing.secretKeyRingFile=你的私钥文件路径

# ossrh账号信息，如果设置了SKIP_CREDENTIAL跳过账号校验，则可以不填
ossrh.username=你账号的用户名
ossrh.password=你账号的密码
```
最后在命令行执行gradle任务发布组件，如果你发布的是android组件(aar)，执行的任务的名称格式为`publish{flavorName}LibraryPublicationToMavenRepository`或`publish{flavorName}LibraryPublicationToMavenLocal`，其中flavorName为android组件的flavor的名称，首字母大写，没有则不填flavorName，如果你发布的是java组件(jar)，执行的任务名称为`publishJarPublicationToMavenRepository`或`publishJarPublicationToMavenLocal`，如果你想二次打包ARTIFACT_PATH指定的aar或jar组件发布，执行的任务名称为`publishPacklibPublicationToMavenRepository`或`publishPacklibPublicationToMavenLocal`：
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

//发布ARTIFACT_PATH指定的aar或jar到maven本地仓库
gradle publishPacklibPublicationToMavenLocal

//发布ARTIFACT_PATH指定的aar或jar到maven远程release或snapshot仓库
gradle publishPacklibPublicationToMavenRepository

//发布所有aar和jar到maven本地仓库
gradle publishToMavenLocal

//发布所有aar和jar到maven远程release或snapshot仓库
gradle publishAllPublicationsToMavenRepository

//发布所有aar和jar到maven本地仓库和maven远程release或snapshot仓库
gradle publish
```
更详细使用可以查看[demo](https://github.com/rain9155/MavenPublishScript/tree/main/demo)
