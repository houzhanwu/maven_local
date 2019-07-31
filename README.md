# maven_local

 版权声明：本文为高绍臣原创文章，转载请注明出处，本文博客地址：http://blog.csdn.net/gaoshaochen。 https://blog.csdn.net/gaoshaochen/article/details/53985686
自己的网站想要对接支付宝的第三方登录，看了下接入文档，sdk接入的方式比较简单，不过蛋疼的地方是ali的sdk没有提供maven引入，自己不想搭建一个maven服务器，但是又想有一个自己的maven服务器，github是一个不错的选择。

原材料：
github账号一枚。 
新建一个项目，项目名称暂且就叫mvnrepo。 
本地安装并配置好了maven。 
下载支付宝sdkhttps://doc.open.alipay.com/doc2/detail?treeId=54&articleId=103419&docType=1。

制作方式：
打开maven安装目录，修改maven配置文件，将本地仓库，指向一个临时的目录比如说e盘下的mvn目录下。

<localRepository>E:/mvn</localRepository>
1
将阿里的sdkjar拷贝在E盘下面。

安装到本地的maven仓库e盘mvn目录下，打开cmd最好用管理员身份运行下面命令： 
mvn install:install-file -Dfile=E:/alipay-sdk.jar -DgroupId=com.ali.alipay -DartifactId=alipay-sdk -Dversion=20161226110022 -Dpackaging=jar

出现BUILD SUCCESS就ok了。

剩下的就比较简单了，把mvn目录下跟支付宝无关的maven的jar包干掉，然后把这个目录提交到我们github上去。

cd E
cd mvn
git init
git remote add origin https://github.com/${youtcount}/mvnrepo.git
git add *
git commit -m 'init'
git push -u origin master

在项目的pom文件中配置如下信息，即可用maven的方式引入alipay sdk

<repositories>
    <repository>
        <id>mvn-repository</id>
        <name>mvn-repository</name>
        <url>https://raw.github.com/${youtcount}/mvnrepo/</url>
    </repository>
</repositories>
<dependency>
    <groupId>com.ali.alipay</groupId>
    <artifactId>alipay-sdk</artifactId>
    <version>20161226110022</version>
</dependency>
--------------------- 
作者：高绍臣 
来源：CSDN 
原文：https://blog.csdn.net/idlear/article/details/53985686 
版权声明：本文为博主原创文章，转载请附上博文链接！