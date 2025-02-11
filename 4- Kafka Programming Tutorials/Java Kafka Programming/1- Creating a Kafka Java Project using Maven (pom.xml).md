Creating a Kafka Java Project using Maven (pom.xml)
===================================================

Learn how to create a Java project using Maven that can interact with Kafka cluster

* * *

Maven is a popular choice for Kafka projects in Java.
-----------------------------------------------------

[](#Maven-is-a-popular-choice-for-Kafka-projects-in-Java.--0)

Before developing Kafka producers and consumers in Java, we'll have to set up a simple Kafka Java project that includes common dependencies that we'll need, namely:

*   Kafka dependencies
    
*   Logging dependencies
    

Follow these steps to create a Java project with the above dependencies.

* * *

Creating a Maven project with pom.xml and setting up dependencies
-----------------------------------------------------------------

[](#Creating-a-Maven-project-with-pom.xml-and-setting-up-dependencies-1)

In IntelliJ IDEA, create a new Java maven project (File > New > Project)

![Kafka Maven Java Project screenshot from IntelliJ IDEA when beginning a new Java Maven Project for interacting with a Kafka Cluster.](../../static/images/image__40_.webp "Creating a new Kafka Maven Java Project file in IntelliJ IDEA")

Then add your Maven project attributes

![Screenshot showing the attributes for your Kafka Demo Java project in Maven ((pom.xl) via IntelliJ IDEA.](../../static/images/image__41_.webp "Adding Your Kafka Maven Project attributes")

The build tool Maven contains a `**pom.xml**` file. The `pom.xml` is a default XML file that carries all the information regarding the GroupID, ArtifactID, as well as the Version values. The user needs to define all the necessary project dependencies in the `pom.xml` file. Go to the `pom.xml` file.

![Defining the dependencies for Kafka Maven Client project in pom.xl xml file screenshot.](../../static/images/image__42_.webp "Kafka Maven (pom.xl) Project XML File")

pom.xl

Define the Kafka Dependencies. Create a `**<dependencies>...</dependencies>**` block within which we will define the required dependencies.

Add a dependency for Kafka client as shown below

```
<project>
    ...

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients -->
        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>2.8.1</version>
        </dependency>
    </dependencies>

</project>
```

If the version number appears red in color, it means the user missed to enable the '**Auto-Import**' option. If so, go to **View > Tool Windows > Maven**. A Maven Projects Window will appear on the right side of the screen. Click on the 'Refresh' button appearing right there. This will enable the missed Auto-Import Maven Projects. If the color changes to black, it means the missed dependency is downloaded.

![Screenshot showing the Maven Projects window on the right for our Kafka Maven project.](../../static/images/image__43_.webp "Kafka Maven Project Window")

Add another dependency for logging. This will enable us to print diagnostic logs while our application runs.

```
        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.32</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-simple -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.32</version>
        </dependency>
```

Now, we have set all the required dependencies. Let's try the _Simple Hello World_ example.

* * *

Creating your first class
-------------------------

[](#Creating-your-first-class-2)

Create a java package say, `io.conduktor.demos.kafka.HelloWorld`

![Screenshot showing how to create a new java class for our Kafka Maven project.](../../static/images/image__44_.webp "Creationg a New Java Class for our Kafka Maven Client Project")

While creating the java package, follow the package naming conventions. Finally, create the sample application program as shown below.

```
package io.conduktor.demos.kafka;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
    private static final Logger log = LoggerFactory.getLogger(HelloWorld.class);

    public static void main(String[] args) {
        log.info("Hello World");
    }
}
```

Run the application (the play green button on line 9 in the screenshot below) and verify that it runs and prints the message, and exits with code `0`. This means that your Java application has run successfully.

Expand the 'External Libraries' on the Project panel and verify that it displays the dependencies that we added for the project in `pom.xml`.

![Screenshot from IntelliJ showing that we have created a sample Kafka Maven Java project that includes all required dependencies. ](../../static/images/image__45_.webp "Sample Kafka Maven Java Project has been created")

All good!

We have created a sample Java project that includes all the needed dependencies. This will form the basis for creating Java producers and consumers next.

---
Next: [Creating a Kafka Java Project using Gradle (build.gradle)](https://github.com/AbdoMusk/Apache-Kafka/blob/main/4-%20Kafka%20Programming%20Tutorials/Java%20Kafka%20Programming/2-%20Creating%20a%20Kafka%20Java%20Project%20using%20Gradle%20%28build.gradle%29.md)