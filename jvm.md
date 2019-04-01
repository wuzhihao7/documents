# jps

jps 命令类似与 linux 的 ps 命令，但是它只列出系统中所有的 Java 应用程序。 通过 jps 命令可以方便地查看 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息。

#### 参数说明

-q：只输出进程 ID
-m：输出传入 main 方法的参数
-l：输出完全的包名，应用主类名，jar的完全路径名
-v：输出jvm参数
-V：输出通过flag文件传递到JVM中的参数

1. **jps**：显示进程id和启动类的名称。

2. **jps -q**：只显示进程id，不显示类的名称

3. **jps -m**：显示传递给main方法的参数

4. **jps -l**：显示主函数的全限定类名

5. **jps -v**：显示传递给Java虚拟机的参数

6. 获取远程服务器jps信息

   jps 支持查看远程服务上的 jvm 进程信息。如果需要查看其他机器上的 jvm 进程，需要在待查看机器上启动 jstatd 服务。

   - 开启jstatd服务

     启动 jstatd 服务，需要有足够的权限。 需要使用 Java 的安全策略分配相应的权限。

     - 创建 jstatd.all.policy 策略文件。

     ```
     grant codebase "file:${java.home}/../lib/tools.jar" {
         permission java.security.AllPermission;
     };
     ```

     - 启动 jstatd 服务器

     ```
     jstatd -J-Djava.security.policy=jstatd.all.policy -J-Djava.rmi.server.hostname=192.168.31.241
     ```

     -J 参数是一个公共的参数，如 jps、 jstat 等命令都可以接收这个参数。 由于 jps、 jstat 命令本身也是 Java 应用程序， -J 参数可以为 jps 等命令本身设置 Java 虚拟机参数。

     -Djava.security.policy：指定策略文件
     -Djava.rmi.server.hostname：指定服务器的ip地址（可忽略）

     默认情况下， jstatd 开启在 1099 端口上开启 RMI 服务器。

     ```
     jps 192.168.179.130
     ```

# jstack

jstack是jdk自带的线程堆栈分析工具，使用该命令可以查看或导出 Java 应用程序中线程堆栈信息。

#### 参数说明：

- -l  长列表. 打印关于锁的附加信息,例如属于java.util.concurrent 的 ownable synchronizers列表.
- -F  当’jstack [-l] pid’没有相应的时候强制打印栈信息
- -m  打印java和native c/c++框架的所有栈信息.
- -h | -help  打印帮助信息

pid 需要被打印配置信息的java进程id,可以用jps查询.