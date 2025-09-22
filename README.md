Below is translated by AI.

Fixed some bugs, optimized and added some features, such as slink points, gadget chains, function annotation recognition, parameter annotation recognition, etc. Also added a web project scanning strategy, where the source point is the route entry. Use the following command:
```
--config
webservice
--noTaintTrack
--maxChainLength
16
--similarLevel
4
--maxRepeatBranchesTimes
10
--skipSourcesFile
/myGadgetinspector/webservice-skip-sources.demo
/temp/halo.jar
```
Parameter description:

--similarLevel n: Solves path explosion, i.e., the problem of too high repetition rate in intermediate paths. For each chain, it takes its first n chains and the last 1 chain as deduplication factors. If there are duplicates, the shortest chain is taken.

--maxRepeatBranchesTimes n: Indicates how many times a certain branch function can appear in all gadget chains at most. Default is 20.

File description:
webservice-skip-sources.demo is the list of route classes to be ignored.

For example, the scan result of the open-source project halo is as follows (the following gadget chain has a directory traversal vulnerability, which has been reported to the vendor):
```
Using classpath: [/temp/halo.jar]
run/halo/app/controller/admin/api/BackupController.getMarkdownBackup(Ljava/lang/String;)Lrun/halo/app/model/dto/BackupDTO; (0)
  java/nio/file/Paths.get(Ljava/lang/String;[Ljava/lang/String;)Ljava/nio/file/Path; (0)

run/halo/app/controller/admin/api/BackupController.getDataBackup(Ljava/lang/String;)Lrun/halo/app/model/dto/BackupDTO; (0)
  java/nio/file/Paths.get(Ljava/lang/String;[Ljava/lang/String;)Ljava/nio/file/Path; (0)

run/halo/app/controller/admin/api/BackupController.getWorkDirBackup(Ljava/lang/String;)Lrun/halo/app/model/dto/BackupDTO; (0)
  java/nio/file/Paths.get(Ljava/lang/String;[Ljava/lang/String;)Ljava/nio/file/Path; (0)

javax/xml/ws/handler/MessageContext$Scope.valueOf(Ljava/lang/String;)Ljavax/xml/ws/handler/MessageContext$Scope; (0)
  java/lang/Enum.valueOf(Ljava/lang/Class;Ljava/lang/String;)Ljava/lang/Enum; (0)
  java/lang/Class.enumConstantDirectory()Ljava/util/Map; (0)
  java/lang/Class.getEnumConstantsShared()[Ljava/lang/Object; (0)
  java/lang/reflect/Method.invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object; (0)

com/sun/xml/internal/ws/handler/HandlerProcessor.closeHandlers(Ljavax/xml/ws/handler/MessageContext;II)V (0)
  java/util/logging/Logger.log(Ljava/util/logging/Level;Ljava/lang/String;Ljava/lang/Throwable;)V (0)
  java/util/logging/Logger.doLog(Ljava/util/logging/LogRecord;)V (0)
  java/util/logging/Logger.getEffectiveLoggerBundle()Ljava/util/logging/Logger$LoggerBundle; (0)
  java/util/logging/Logger.findResourceBundle(Ljava/lang/String;Z)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.getBundle(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/ClassLoader;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.getBundleImpl(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/ClassLoader;Ljava/util/ResourceBundle$Control;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.findBundle(Ljava/util/ResourceBundle$CacheKey;Ljava/util/List;Ljava/util/List;ILjava/util/ResourceBundle$Control;Ljava/util/ResourceBundle;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.findBundleInCache(Ljava/util/ResourceBundle$CacheKey;Ljava/util/ResourceBundle$Control;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle$Control.needsReload(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/String;Ljava/lang/ClassLoader;Ljava/util/ResourceBundle;J)Z (0)
  java/net/URL.openConnection()Ljava/net/URLConnection; (0)

com/sun/xml/internal/ws/handler/HandlerTube.closeServersideHandlers(Ljavax/xml/ws/handler/MessageContext;)V (0)
  com/sun/xml/internal/ws/handler/HandlerProcessor.closeHandlers(Ljavax/xml/ws/handler/MessageContext;II)V (0)
  java/util/logging/Logger.log(Ljava/util/logging/Level;Ljava/lang/String;Ljava/lang/Throwable;)V (0)
  java/util/logging/Logger.doLog(Ljava/util/logging/LogRecord;)V (0)
  java/util/logging/Logger.getEffectiveLoggerBundle()Ljava/util/logging/Logger$LoggerBundle; (0)
  java/util/logging/Logger.findResourceBundle(Ljava/lang/String;Z)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.getBundle(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/ClassLoader;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.getBundleImpl(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/ClassLoader;Ljava/util/ResourceBundle$Control;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.findBundle(Ljava/util/ResourceBundle$CacheKey;Ljava/util/List;Ljava/util/List;ILjava/util/ResourceBundle$Control;Ljava/util/ResourceBundle;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle.findBundleInCache(Ljava/util/ResourceBundle$CacheKey;Ljava/util/ResourceBundle$Control;)Ljava/util/ResourceBundle; (0)
  java/util/ResourceBundle$Control.needsReload(Ljava/lang/String;Ljava/util/Locale;Ljava/lang/String;Ljava/lang/ClassLoader;Ljava/util/ResourceBundle;J)Z (0)
  java/net/URL.openConnection()Ljava/net/URLConnection; (0)

```

## threedr3am版本https://github.com/threedr3am/gadgetinspector
================
##### 1. Added Fastjson gadget chain discovery

Usage: Start with the main method, startup parameters:
```
--config fastjson /xxxxx/xxxxx/xxxxx/xxxxx.jar /xxxxx/xxxxx/xxxxx/xxxxx2.jar /xxxxx/xxxxx/xxxxx/xxxxx3.jar
```

##### 2. Added SQLInject detection

Currently, slink only includes detection for JdbcTemplate. Mybatis, native JDBC, JPA, Hibernate, etc. will be added gradually.

Usage: Start with the main method, startup parameters (newly added --boot parameter, used when the Spring Boot project jar contains other jar dependencies):
```
--config sqlinject --boot /xxxxx/xxxx/jdbc-1.0-SNAPSHOT.jar
```

#### 3. No Taint Analysis (Discover more comprehensive chains)

Recently added --NoTaintTrack parameter, which specifies not to use taint analysis. All chains will be searched. The advantage is that nothing will be missed, the disadvantage is that a lot of manual auditing is required.

Usage: Start with the main method, startup parameters:
```
--config fastjson --boot --noTaintTrack /xxxxx/xxxxx/xxxxx/xxxxx.jar
```
Recommended usage:
```
--config fastjson
--noTaintTrack
--craw 0
--max 30
--history scan-history-fastjson-jndi.dat
--slink JNDI
--skipSourcesFile /Users/threedr3am/xxx/gadgetinspector/fastjson-skip-sources.demo
/Users/threedr3am/.m2/repository/
```
Traverse the /Users/threedr3am/.m2/repository/ directory, and for each batch of 30 jars found, use no taint analysis to discover Fastjson gadgets with JNDI slink.

#### Parameter description
1. --config xxx: What kind of gadget chains to discover (jackson, fastjson, sqlinject, jserial...)
2. --boot: Specify that this jar is a Spring Boot project jar
3. --noTaintTrack: Do not use taint analysis, all chains will be searched. The advantage is nothing will be missed, the disadvantage is a lot of manual auditing is required
4. --mybatis.xml xxx: When discovering sqlinject, if the project uses Mybatis, you can specify the directory where the mapper xml is located to discover Mybatis SQL injection
5. --resume: Whether to not delete all dat data files when the project starts
6. --opLevel 1: Chain aggregation optimization level, 1 means one level of optimization, default is 0 (no optimization)
7. --history recordFileName: Enable historical scan jar record, to avoid scanning old jars repeatedly during large-scale scanning. The advantage is reduced work time, the disadvantage is that gadgets composed of dependencies may not be found
8. --max 100: Scan up to 100 jar files
10. --onlyJDK: Only scan JDK dependencies (rt.jar, jce.jar)
11. --maxChainLength 5: Only output chains with length less than or equal to 5
12. --crawMaven /Users/threedr3am/jar/: Use the built-in crawler to automatically crawl the Maven repository, jars are stored in /Users/threedr3am/jar/. Maven repository is very slow, you can use a proxy
13. --onlyCrawMaven: Only start the Maven crawler
14. --onlyCrawMavenPopular: Only start the maven-popular crawler
15. --onlyCrawNexus: Only start the Nexus crawler
16. --craw 10: Enable crawler function, analyze every 10 minutes and generate a report. If --crawMaven is configured, use the built-in crawler to crawl the Maven repository
17. --slink JNDI: Specify the slinks to discover, options are JNDI, SSRFAndXXE, EXEC, FileIO, Reflect, BCEL (for Hessian only). By default, all slinks except the special ones are discovered
18. --skipSourcesFile /xxx/xxxx/xxx.txt: Skip class sources that are often false positives, refer to the file fastjson-skip-sources.demo
19. --slinksFile /xxx/xxxx/xxx.txt: Customize the slinks to discover. When this is used, the --slink parameter is ignored. Refer to the file fastjson-slinks.demo

## JackOfMostTrades版本https://github.com/JackOfMostTrades/gadgetinspector
================

This project inspects Java libraries and classpaths for gadget chains. Gadgets chains are used to construct exploits for deserialization vulnerabilities. By automatically discovering possible gadgets chains in an application's classpath penetration testers can quickly construct exploits and application security engineers can assess the impact of a deserialization vulnerability and prioritize its remediation.

This project was presented at Black Hat USA 2018. Learn more about it there! (Links pending)

DISCLAIMER: This project is alpha at best. It needs tests and documentation added. Feel free to help by adding either!

Building
========

Assuming you have a JDK installed on your system, you should be able to just run `./gradlew shadowJar`. You can then run the application with `java -jar build/libs/gadget-inspector-all.jar <args>`.
 
How to Use
==========

This application expects as argument(s) either a path to a war file (in which case the war will be exploded and all of its classes and libraries used as a classpath) or else any number of jars.

Note that the analysis can be memory intensive (and so far gadget inspector has not been optimized at all to be less memory greedy). For small libraries you probably want to allocate at least 2GB of heap size (i.e. with the `-Xmx2G` flag). For larger applications you will want to use as much memory as you can spare.

The toolkit will go through several stages of classpath inspection to build up datasets for use in later stages. These datasets are written to files with a `.dat` extension and can be discarded after your run (they are written mostly so that earlier stages can be skipped during development).

After the analysis has run the file `gadget-chains.txt` will be written.


Example
=======

The following is an example from running against [`commons-collections-3.2.1.jar`](http://central.maven.org/maven2/commons-collections/commons-collections/3.2.1/commons-collections-3.2.1.jar), e.g. with

```
wget http://central.maven.org/maven2/commons-collections/commons-collections/3.2.1/commons-collections-3.2.1.jar
java -Xmx2G -jar build/libs/gadget-inspector-all.jar commons-collections-3.2.1.jar
```

In gadget-chains.txt there is the following chain:
```
com/sun/corba/se/spi/orbutil/proxy/CompositeInvocationHandlerImpl.invoke(Ljava/lang/Object;Ljava/lang/reflect/Method;[Ljava/lang/Object;)Ljava/lang/Object; (-1)
  com/sun/corba/se/spi/orbutil/proxy/CompositeInvocationHandlerImpl.invoke(Ljava/lang/Object;Ljava/lang/reflect/Method;[Ljava/lang/Object;)Ljava/lang/Object; (0)
  org/apache/commons/collections/map/DefaultedMap.get(Ljava/lang/Object;)Ljava/lang/Object; (0)
  org/apache/commons/collections/functors/InvokerTransformer.transform(Ljava/lang/Object;)Ljava/lang/Object; (0)
  java/lang/reflect/Method.invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object; (0)
```

The entry point of this chain is an implementation of the JDK `InvocationHandler` class. Using the same trick as in the original commons-collections gadget chain, any serializable implementation of this class is reachable in a gadget chain, so the discovered chain starts here. This method invokes `classToInvocationHandler.get()`. The discovered gadget chain indicates that the `classToInvocationHandler` can be serialized as a `DefaultedMap` so that the this invocation jumps to `DefaultedMap.get()`. The next step in the chain invokes `value.transform()` from this method. The parameter `value` in this class can be serialized as a `InvokerTransformer`. Inside this class's `transform` method we see that we call `cls.getMethodName(iMethodName, ...).invoke(...)`. Gadget inspector determined that `iMethodName` is attacker controllable as a serialized member, and thus an attacker can execute an arbitrary method on the class.
 
This gadget chain is the building block of the [full commons-collections gadget](https://github.com/frohoff/ysoserial/blob/master/src/main/java/ysoserial/payloads/CommonsCollections1.java) chain discovered by Frohoff. In the above case, the gadget inspector happened to discovery entry through `CompositeInvocationHandlerImpl` and `DefaultedMap` instead of `AnnotationInvocationHandler` and `LazyMap`, but is largely the same.


Other Examples
==============

If you're looking for more examples of what kind of chains this tool can find, the following libraries also have some interesting results:

* http://central.maven.org/maven2/org/clojure/clojure/1.8.0/clojure-1.8.0.jar
* https://mvnrepository.com/artifact/org.scala-lang/scala-library/2.12.5
* http://central.maven.org/maven2/org/python/jython-standalone/2.5.3/jython-standalone-2.5.3.jar

Don't forget that you can also point gadget inspector at a complete application (packaged as a JAR or WAR). For example, when analyzing the war for the [Zksample2](https://sourceforge.net/projects/zksample2/) application we get the following gadget chain:

```
net/sf/jasperreports/charts/design/JRDesignPieDataset.readObject(Ljava/io/ObjectInputStream;)V (1)
  org/apache/commons/collections/FastArrayList.add(Ljava/lang/Object;)Z (0)
  java/util/ArrayList.clone()Ljava/lang/Object; (0)
  org/jfree/data/KeyToGroupMap.clone()Ljava/lang/Object; (0)
  org/jfree/data/KeyToGroupMap.clone(Ljava/lang/Object;)Ljava/lang/Object; (0)
  java/lang/reflect/Method.invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object; (0)
```

As you can see, this utilizes several different libraries contained in the application in order to build up the chain.

FAQ
===

**Q:** If gadget inspector finds a gadget chain, can an exploit be built from it?

**A:** Not always. The analysis uses some simplifying assumptions and can report false positives (gadget chains that don't actually exist). As a simple example, it doesn't try to solve for the satisfiability of branch conditions. Thus it will report the following as a gadget chain:

```java
public class MySerializableClass implements Serializable {
    public void readObject(ObjectInputStream ois) {
        if (false) System.exit(0);
        ois.defaultReadObject();
    }
}
```

Furthermore, gadget inspector has pretty broad conditions on those functions it considers interesting. For example, it treats reflection as interesting (i.e. calls to `Method.invoke()` where an attacker can control the method), but often times overlooked assertions mean that an attacker can *influence* the method invoked but does not have complete control. For example, an attacker may be able to invoke the "getError()" method in any class, but not any other method name.


**Q:** If no gadget chains were found, does that mean my application is safe from exploitation?

**A:** No! For one, the gadget inspector has a very narrow set of "sink" functions which it considers to have "interesting" side effects. This certainly doesn't mean there aren't other interesting or dangerous behaviors not in the list.

Furthermore, there are a number of limitations to static analysis that mean the gadget inspector will always have blindspots. As an example, gadget inspector would presently miss this because it doesn't follow reflection calls.

```java
public class MySerializableClass implements Serializable {
    public void readObject(ObjectInputStream ois) {
        System.class.getMethod("exit", int.class).invoke(null, 0);
    }
}
```
