POM中没有指定打包类型，使用默认打包类型jar。

执行命令`mvn clean package`进行打包
Maven会在打包之前执行编译、测试等操作。这里jar:jar任务负责打包，实际上就是jar插件的jar目标将项目主代码打包成一个名为hello-world-1.0-SNAP-SHOT.jar的文件。该文件也位于target/输出目录中，它是根据artifact-version.jar规则进行命名的，如有需要，还可以使用finalName来自定义该文件的名称。

让其他的Maven项目直接引用这个jar呢？还需要一个安装的步骤，执行`mvn clean install`
在打包之后，又执行了安装任务install:install。从输出可以看到该任务将项目输出的jar安装到了Maven本地仓库中

默认打包生成的jar是不能够直接运行的，因为带有main方法的类信息不会添加到manifest中（打开jar文件中的META-INF/MANIFEST.MF文件，将无法看到Main-Class一行）。为了生成可执行的jar文件，需要借助maven-shade-plugin
