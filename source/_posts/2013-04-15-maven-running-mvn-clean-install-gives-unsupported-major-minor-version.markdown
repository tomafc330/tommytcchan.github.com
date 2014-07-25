---
layout: post
title: "Maven: running mvn clean install gives unsupported major minor version."
date: 2013-04-15 22:00
comments: true
categories: [Java,Maven,problems]
---
I ran into this problem when I was building a maven project:
```
Fatal error during compilation org.apache.tools.ant.BuildException: java.lang.UnsupportedClassVersionError: com/sun/tools/javac/Main : Unsupported major.minor version 51.0 (Use --stacktrace to see the full trace)
```

Want to know the reason? It's because my JAVA_HOME dir has java 7 defined while the output of ```java -version``` points to a java 6 installation. So fix this on Ubuntu:

```
sudo update-alternatives --config java
```

Choose the correct java version and you're done!

