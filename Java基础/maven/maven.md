Maven坐标的元素包括groupId、artifactId、version、packaging、classifier。
- groupId：定义当前Maven项目隶属的实际项目。
  groupId的表示方式与Java包名的表示方式类似，通常与域名反向一一对应。

- artifactId：该元素定义实际项目中的一个Maven项目（模块），推荐的做法是使用实际项目名称作为artifactId的前缀。
  在默认情况下，Maven生成的构件，其文件名会以artifactId作为开头

- version：该元素定义Maven项目当前所处的版本

- packaging：该元素定义Maven项目的打包方式。
  打包方式通常与所生成构件的文件扩展名对应
  打包方式会影响到构建的生命周期，比如jar打包和war打包会使用不同的命令。最后，当不定义packaging的时候，Maven会使用默认值jar。

- classifier：该元素用来帮助定义构建输出的一些附属构件。附属构件与主构件对应，如上例中的主构件是nexus-indexer-2.0.0.jar，该项目可能还会通过使用一些插件生成如nexus-indexer-2.0.0-javadoc.jar、nexus-indexer-2.0.0-sources.jar这样一些附属构件，其包含了Java文档和源代码。这时候，javadoc和sources就是这两个附属构件的classifier。这样，附属构件也就拥有了自己唯一的坐标。还有一个关于classifier的典型例子是TestNG,TestNG的主构件是基于Java 1.4平台的，而它又提供了一个classifier为jdk5的附属构件。
  注意，不能直接定义项目的classifier，因为附属构件不是项目直接默认生成的，而是由附加的插件帮助生成。

groupId、artifactId、version是必须定义的，packaging是可选的（默认为jar），而classifier是不能直接定义的。

Maven内置了一个中央仓库的地址（http://repo1.maven.org/maven2）



```sh
mvn archetype:generate "-DgroupId=com.companyname.bank" "-DartifactId=consumerBanking" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"
```
POM中没有指定打包类型，使用默认打包类型jar。

执行命令`mvn clean package`进行打包
Maven会在打包之前执行编译、测试等操作。这里jar:jar任务负责打包，实际上就是jar插件的jar目标将项目主代码打包成一个名为hello-world-1.0-SNAP-SHOT.jar的文件。该文件也位于target/输出目录中，它是根据artifact-version.jar规则进行命名的，如有需要，还可以使用finalName来自定义该文件的名称。

让其他的Maven项目直接引用这个jar呢？还需要一个安装的步骤，执行`mvn clean install`
在打包之后，又执行了安装任务install:install。从输出可以看到该任务将项目输出的jar安装到了Maven本地仓库中

默认打包生成的jar是不能够直接运行的，因为带有main方法的类信息不会添加到manifest中（打开jar文件中的META-INF/MANIFEST.MF文件，将无法看到Main-Class一行）。为了生成可执行的jar文件，需要借助maven-shade-plugin
```xml

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.4</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <transformers>
                <transformer
                  implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                  <mainClass>com.juvenxu.mvnbook.helloworld.HelloWorld</mainClass>
                </transformer>
              </transformers>
            </configuration>
          </execution>
        </executions>
      </plugin>
```


# 依赖的配置

根元素project下的dependencies可以包含一个或者多个dependency元素，以声明一个或者多个项目依赖。每个依赖可以包含的元素有：groupId、artifactId和version：依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven根据坐标才能找到需要的依赖。
- type：依赖的类型，对应于项目坐标定义的packaging。大部分情况下，该元素不必声明，其默认值为jar。
- scope：依赖的范围，见5.5节。
- optional：标记依赖是否可选，见5.8节。
- exclusions：用来排除传递性依赖，见5.9.1节。

## 依赖的范围

### 三种classpath 

编译classpath、测试classpath、运行classpath

1. Maven在**编译**项目主代码的时候需要使用一套classpath。在上例中，编译项目主代码的时候需要用到spring-core，该文件以依赖的方式被引入到classpath中。

2. 其次，Maven在编译和执行测试的时候会使用另外一套classpath。上例中的JUnit就是一个很好的例子，该文件也以依赖的方式引入到测试使用的classpath中，不同的是这里的依赖范围是test。

3. 最后，实际**运行**Maven项目的时候，又会使用一套classpath，上例中的spring-core需要在该classpath中，而JUnit则不需要。

依赖范围就是用来控制依赖与三种classpath的关系，Maven有以下几种依赖范围：

- compile：编译依赖范围。如果没有指定，就会默认使用该依赖范围。使用此依赖范围的Maven依赖，对于编译、测试、运行三种classpath都有效。典型的例子是spring-core，在编译、测试和运行的时候都需要使用该依赖。

- test：测试依赖范围。使用此依赖范围的Maven依赖，只对于测试classpath有效，在编译主代码或者运行项目的使用时将无法使用此类依赖。典型的例子是JUnit，它只有在编译测试代码及运行测试的时候才需要。

- provided：已提供依赖范围。使用此依赖范围的Maven依赖，对于编译和测试class-path有效，但在运行时无效。典型的例子是servlet-api，编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供，就不需要Maven重复地引入一遍。

- runtime：运行时依赖范围。使用此依赖范围的Maven依赖，对于测试和运行class-path有效，但在编译主代码时无效。典型的例子是JDBC驱动实现，项目主代码的编译只需要JDK提供的JDBC接口，只有在执行测试或者运行项目的时候才需要实现上述接口的具体JDBC驱动。

- system：系统依赖范围。该依赖与三种classpath的关系，和provided依赖范围完全一致。但是，使用system范围的依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植，因此应该谨慎使用。systemPath元素可以引用环境变量，如：

- import（Maven 2.0.9及以上）：导入依赖范围。该依赖范围不会对三种classpath产生实际的影响

依赖范围与classpath的关系

| 依赖范围(scope) |     | |
|----------|------|
| dsa      | asdf |
| asdfdfas | asdf |


