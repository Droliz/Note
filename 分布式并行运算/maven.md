# maven

### 配置maven

下载maven后，在系统环境变量中添加下载的目录，在用户环境变量中添加bin目录

系统环境变量
```
MAVEN_HOME
maven根目录
```

用户环境变量
```
%MAVEN_HOME%\bin
```

### 配置setting.xml

找到根目录下的`config\setting.xml`，配置本地仓库

```xml
<localRepository>PATH</localRepository>
```

配置镜像源

```xml
<mirror>
  <id>alimaven</id>
  <mirrorOf>central</mirrorOf>
  <name>aliyun maven</name>
  <url>https://maven.aliyun.com/repository/central</url>
</mirror>
```

配置jdk

```xml
<profile>
  <id>jdk-1.8</id>

  <activation>
	<jdk>1.8</jdk>
  </activation>

  <properties>
	<maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
	<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>
```

### 配置IDEA中的Maven

在IDEA的`文件->设置->构建、执行、部署->构建工具->Maven`中选择Maven主路径为本地的Maven路径

