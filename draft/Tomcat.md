![img](file:///C:/Users/lyuweigh/Documents/My Knowledge/temp/60aaf6aa-04cb-47ee-949b-4c3780e51c73/128/index_files/cae8459e-598a-4387-ab78-8a9ff5b1d3c4.png)





\### Tomcat安装步骤

1. 配置环境变量JAVA_HOME,Tomcat会区去读JAVA_HOME里面的信息

2. 目录结构

![img](file:///C:/Users/lyuweigh/Documents/My%20Knowledge/temp/60aaf6aa-04cb-47ee-949b-4c3780e51c73/128/index_files/b5bf45fb-b929-4059-893e-967a2e3e9947.jpg)

```
bin:脚本目录

   启动脚本:startup.bat

   停止脚本:shutdown.bat

   在Linux环境下则是以.sh结尾的

   并且linux下的.sh文件需要赋予权限后才能执行,否则会抛出找不到命令的错误



conf:配置文件目录

    核心配置文件:server.xml (这个里面可以配置)自己映射的文件内容

    <!--自己添加<Context path="/lyuweigh" docBase="F:\test" debug="0"/>内容-->

    用户权限配置文件:tomcat-users.xml
    

tomcat初始化文件:web.xml
 lib:依赖库

    存放Web项目需要的依赖(其实也就是一些要被加载的源码文件)





 log:日志文件

    localhost_access_log..txt tomcat记录用户访问信息，星表示时间。




 temp:临时文件目录,文件夹内容可以随意删除



 webapps:默认的项目发布路径



 work:tomcat处理Jsp的工作目录


```





