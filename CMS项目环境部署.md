# 开发项目环境部署文档
以CMS项目环境部署为例
## 1.代码获取
### 1.1.GitLab Clone
+ 登录 http://gitlab.61info.com/clone 以下项目
+ RBKD-CMS（项目文件） 
  1. i61-common（项目依赖源码） 
  2. RBKD-MODULE（项目依赖源码）
### 1.2 安装Git
安装TortoiseGit,配置Git安装路径，与Git相关联
在想要拷贝项目的目录下右键TortoiseGit并克隆，填入Git的Http路径即可拷贝相应目录，也可在git bash里使用指令拷贝相应目录

## 2.双IP环境配置
### 2.1.Ipv4环境配置
网络与Internet设置中更改适配器选项，选择IPv4协议进行属性修改

### 2.2.本地DNS服务配置
在C:\Windows\System32\drivers\etc\hosts，配置好本地dns服务

## 3.下载并安装JDK
## 4.下载并安装TOMCAT
## 5.下载并安装NGINX
## 6.项目部署
### 6.1.IDE导入项目
+ 在 IDE 中导入项目代码及依赖项目
1. 导入Project    RBKD-CMS（项目文件）
2. 导入 Module   i61-common（项目依赖源码）
3. 导入 Module   RBKD-MODULE（项目依赖源码）
### 6.2.IDE 中设置依赖包使用源码
-右键项目-路径设置，将两个依赖源码加入RBKD-CMS项目文件中
-安装maven,配置环境变量，设置本地仓库，在IDEA中设置Maven目录，设置阿里云镜像。
-maven 配置文件中加入
```
<profile>
    <id>local_nexus</id>
      <repositories>
        <repository>
          <id>nexus</id>
          <name>Nexus</name>
          <url>http://nexus.server.i61.com:8081/nexus/content/groups/public/</url>
            <releases>
			<enabled>true</enabled>
		  </releases>
          <snapshots>
			<enabled>true</enabled>
		  </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>local_nexus</activeProfile>
  </activeProfiles>
<plugin>
```
### 6.3.在 IDE 中关联 tomcat 并启动
+ 关联tomcat,在IDEA中设置tomcat所在目录
+ 查看tomcat本地jar包是否缺失
+ 设置端口号为8081
+ 设置Application context 路径为/RBKD-CMS/（eclipse在项目属性中设置上下文路径为/，因为在LoginController里的跳转是直接跳转到"/"下的，没有配置EL路径变量）

## 7.项目访问
### 7.1.启动nginx
编辑nginx.bat,修改NGINX_DIR路径为你的nginx目录
双击.bat文件启动nginx
### 7.2.Conf配置更改
编辑nginx.conf，修改反向代理指向的url

### 7.3.更改本地DNS
找到C:\Windows\System32\drivers\etc\hosts
配置本地DNS关联
127.0.0.1       dev.cms.61info.cn
将dev.cms.61info.cn 改为127.0.0.1
### 7.4.输入URL进入系统
在浏览器中输入 dev.cms.61info.cn
+ 输入账号密码，进入CMS系统
**注意：根据同源策略，127.0.0.1与localhost是不同源的，所以登录时保存的信息的session的sessionId与后一个请求的sessionId不一致，导致无法获取之前保存的信息**