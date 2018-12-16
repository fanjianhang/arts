# 第三周

## Tip

- mvn package 和 install的区别

[stackoverflow的答案](https://stackoverflow.com/questions/16602017/how-are-mvn-clean-package-and-mvn-clean-install-different)

package命令会编译你的代码并且打包，比如pom文件中指明jar的打包方式，执行命令后怎会在目标文件目录中生成对应的jar包。

install命令会编译你的代码并且打包，但是这个命令还会把package放到你的本地maven仓库中，其他别的项目就可以引用这个package了。

