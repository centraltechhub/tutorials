
1. Create a new directory called mavenproject.

2. Create a new file called pom.xml with the following content.

**pom.xml**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>custom.application</groupId>
    <artifactId>hello</artifactId>
    <version>SNAPSHOT-1.0</version>
    <properties>
        <java.version>18</java.version>
    </properties>
    <build>
        <plugins>
            <plugin>    
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**Java code**

Under maveproject directory, create the following folder structure: `/src/main/java/custom/application`. Create a java file with sample as name with the following content.

```Java
package custom.application;
public class sample 
{
    public String HelloWorld()
    {
        return "Hello World Jawahar";
    }    
}
```

3. Clean maven project.<br/>
`mvn clean`

4. Package the maven project.<br/>
`mvn package`

5. Install the maven project.<br/>
`mvn install`


