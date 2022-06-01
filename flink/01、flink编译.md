#1.编译步骤
拉代码  ：git clone https://github.com/apache/flink.git  
checkout 到1.14.4分支  

编译：  
mvn clean install -Dmaven.test.skip=true -Dfast -T 4 -Dmaven.compile.fork=true  
cd flink-dist  
mvn clean install



# 2.编译问题
##1）拉不到kafka-schema-registry-client jar包
[ERROR] 
Failed to execute goal on project flink-avro-confluent-registry: Could not resolve dependencies for project org.apache.flink:flink-avro-confluent-registry:jar:1.14.4: Failure to find io.confluent:kafka-schema-registry-client:jar:5.5.2 in https://mirrors.huaweicloud.com/repository/maven/ was cached in the local repository, resolution will not be reattempted until the update interval of huaweicloud has elapsed or updates are forced -> [Help 1]  
解决方法：
手动下载kafka-schema-registry-client:jar:5.5.2和pom，（下载地址：https://packages.confluent.io/maven/io/confluent/kafka-schema-registry-client/5.5.2/），     然后通过以下命令安装到local仓库  
`mvn install:install-file -DgroupId=io.confluent -DartifactId=kafka-schema-registry-client -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/kafka-schema-registry-client-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/kafka-schema-registry-client-5.5.2.pom`
##2）cannot access io.confluent.kafka.serializers.AbstractKafkaSchemaSerDe
原因：
maven 中心仓提供的 kafka-avro-serializer 的 pom 文件没有依赖信息，手动安装依赖或者修改本地 maven 库中的 pom 文件.  
解决方法：  
> mvn install:install-file -DgroupId=io.confluent -DartifactId=kafka-avro-serializer -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/kafka-avro-serializer-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/kafka-avro-serializer-5.5.2.pom  
mvn install:install-file -DgroupId=io.confluent -DartifactId=kafka-schema-serializer -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/kafka-schema-serializer-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/kafka-schema-serializer-5.5.2.pom  
mvn install:install-file -DgroupId=io.confluent -DartifactId=kafka-schema-registry-client -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/kafka-schema-registry-client-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/kafka-schema-registry-client-5.5.2.pom  
mvn install:install-file -DgroupId=io.confluent -DartifactId=kafka-schema-registry-parent -Dversion=5.5.2 -Dpackaging=pom -Dfile=/Users/zhuyinyin/Downloads/kafka-schema-registry-parent-5.5.2.pom -DpomFile=/Users/zhuyinyin/Downloads/kafka-schema-registry-parent-5.5.2.pom  
mvn install:install-file -DgroupId=io.confluent -DartifactId=common -Dversion=5.5.2 -Dpackaging=pom -Dfile=/Users/zhuyinyin/Downloads/common-5.5.2.pom -DpomFile=/Users/zhuyinyin/Downloads/common-5.5.2.pom  
mvn install:install-file -DgroupId=io.confluent -DartifactId=common-parent -Dversion=5.5.2 -Dpackaging=pom -Dfile=/Users/zhuyinyin/Downloads/common-parent-5.5.2.pom -DpomFile=/Users/zhuyinyin/Downloads/common-parent-5.5.2.pom
mvn install:install-file -DgroupId=io.confluent -DartifactId=common-config -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/common-config-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/common-config-5.5.2.pom
mvn install:install-file -DgroupId=io.confluent -DartifactId=common-utils -Dversion=5.5.2 -Dpackaging=jar -Dfile=/Users/zhuyinyin/Downloads/common-utils-5.5.2.jar -DpomFile=/Users/zhuyinyin/Downloads/common-utils-5.5.2.pom





# 参考
https://nightlies.apache.org/flink/flink-docs-release-1.14/zh/docs/flinkdev/building/
https://www.victorchu.info/posts/81f17543/