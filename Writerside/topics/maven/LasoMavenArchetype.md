[使用我们的maven archetype生成项目模板](http://wiki.dev.genebox.cn/pages/viewpage.action?pageId=1343594)

# 方式一（推荐）：

使用maven命令行：

mvn archetype:generate -DgroupId=<GROUP_ID> -DartifactId=<ARTIFACT_ID> -Dpackage=<BASE_PACKAGE> -Dproject=<PROJECT_NAME> -DarchetypeGroupId=com.lasogene.tools -DarchetypeArtifactId=laso-ms-archetype -DarchetypeVersion=1.0.2-SNAPSHOT -U



将需要生成的项目的groupId、artifactId、package和project分别替换上述命令中的<GROUP_ID>、<ARTIFACT_ID>、<BASE_PACKAGE>、<PROJECT_NAME>后，运行命令即可。

**自定义属性package和project，其中package指的是生成项目的基础包名，project是项目在bitbucket中的名称。**



示例：

mvn archetype:generate -DgroupId=com.laso.chip -DartifactId=laso_chip_manager -Dpackage=com.laso.chip -Dproject=laso_chip_manager -DarchetypeGroupId=com.lasogene.tools -DarchetypeArtifactId=laso-ms-archetype -DarchetypeVersion=1.0.2-SNAPSHOT -U

