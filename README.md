#### **MyBatis Generator 简介**
  • 简称MBG，是一个专门为MyBatis框架使用者定 制的代码生成器，可以快速的根据表生成对应的 映射文件，接口，以及bean类。支持基本的增删 改查，以及QBC风格的条件查询。但是表连接、 存储过程等这些复杂sql的定义需要我们手工编写 
 
  • 官方文档地址 http://www.mybatis.org/generator/  
  • 官方工程地址 https://github.com/mybatis/generator/releases
  
在看这篇文章之前建议先去看一下这篇（SSM整合）http://blog.csdn.net/qq_33524158/article/details/78360268 在这篇文章的基础上进行添加的 

#### **正文**

首先在resources 下创建generatorConfig.xml配置文件：

这里每一个配置都有解释认真看下面内容即可在这不做说明，加入如下内容：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
		PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
		"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>

	<!--一定要手动加jar包  否则会报错-->
	<classPathEntry location="D:\\DataSource\\mysql-connector-java-5.1.7-bin.jar"/>
	<context id="DB2Tables" targetRuntime="MyBatis3">


		<!-- 不生成注释 -->
		<commentGenerator>
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!-- 配置数据库连接 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
						connectionURL="jdbc:mysql://localhost:3306/ssm_crud" userId="root"
						password="1010">
		</jdbcConnection>

		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- 指定javaBean生成的位置 -->
		<javaModelGenerator targetPackage="cn.hfbin.crud.beans"
							targetProject=".\src\main\java">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>

		<!--指定sql映射文件生成的位置 -->
		<sqlMapGenerator targetPackage="mapper" targetProject=".\src\main\resources">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>

		<!-- 指定dao接口生成的位置，mapper接口 -->
		<javaClientGenerator type="XMLMAPPER"
							 targetPackage="cn.hfbin.crud.dao" targetProject=".\src\main\java">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>


		<!-- table指定每个表的生成策略
				tableName数据库表名
				domainObjectName生成bean的名字
		-->
		<table tableName="tbl_dept" domainObjectName="Department"></table>
		<table tableName="tbl_emp" domainObjectName="Employee"></table>
		</context>
</generatorConfiguration>

```
注意：自己在resources下创建一名叫mapper的文件夹
注意：如果不另外引入jar包的话等一下项目跑出来会报错

```
<!--一定要手动加jar包  否则会报错-->
	<classPathEntry location="D:\\DataSource\\mysql-connector-java-5.1.7-bin.jar"/>
```

***上面配置文件已写有 无需再复制进去***


不引入将报如下错误：
>  Failed to execute goal org.mybatis.generator:mybatis-generator-maven-plugin:1.3.2:generate (default-cli) on project MyBatisGenerator: Execution default-cli of goal org.mybatis.generator:mybatis-generator-maven-plugin:1.3.2:generate failed: Exception getting JDBC Driver: com.mysql.jdbc.Driver -> [Help 1]
> 

![这里写图片描述](http://img.blog.csdn.net/20171104114826481?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM1MjQxNTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在pom.xml配置文件中加入如下内容：
```
<build>
        <finalName>mybatis_generator</finalName>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.2</version>
                <configuration>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
        </plugins>
 </build>
```

加入如下内容后旁边maven工具会出现一个执行命令

![这里写图片描述](http://img.blog.csdn.net/20171104115628943?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM1MjQxNTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)




到这算配置完了就差运行结果了：

先上张没有运行的目录图
![这里写图片描述](http://img.blog.csdn.net/20171104120201432?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM1MjQxNTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

红色框出来的需要自己建好！！

好下面就开始运行了


运行结果如下：
![这里写图片描述](http://img.blog.csdn.net/20171104120433745?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM1MjQxNTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


看到bean包跟dao包都多了java文件，以及resources下mapper也多xml文件
说明我们的逆向工程运行成功。

好   !!!! 到这我们打开一个xml文件看看

![这里写图片描述](http://img.blog.csdn.net/20171104120949423?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM1MjQxNTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



这里面生成的sql 各种各样（增删改查每个都有两个或两个以上）有很多sql或许是我们不想要的，没关系， 如果你觉得你用不到的可以删掉，也可以自己定义自己的sql语句。

MyBatis-逆向工程  就介绍到这里  
## 感谢各位老哥的阅读


