## Prepare develop environment

Recommand:
- Linux OS
- JDK 7/8
- Maven
- Git
- Thrift 0.9.3
- IntelliJ IDEA


### Maven

Config a mirror for fetching dependencies jars

For missing jars, you can use `mvn install` command to install to your local repository

### Git

Clone Carbondata project from Apache github repository. Recommand to fork the project to your own github accout and clone.


## Build
```
mvn -DskipTests -Pspark-2.2 -Dspark.version=2.2.1 clean package
```
