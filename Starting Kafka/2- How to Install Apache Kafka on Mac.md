How to Install Apache Kafka on Mac?
===================================

How to install and run Kafka on Mac OS X

* * *

> [!TIP]
> **Start Kafka with Zookeeper**
>
> For maximum compatibility purposes, it is recommended to start Apache Kafka with Zookeeper. If you would like to run Kafka without Zookeeper, take a look [here](/kafka/how-to-install-apache-kafka-on-mac-without-zookeeper-kraft-mode/).

How to install Kafka with Zookeeper on Mac
------------------------------------------

[](#How-to-install-Kafka-with-Zookeeper-on-Mac-0)

1.  Install Java JDK version 11
    
2.  Download Apache Kafka from [https://kafka.apache.org/downloads](https://kafka.apache.org/downloads) under 'Binary Downloads'
    
3.  Extract the contents on your Mac
    
4.  Start Zookeeper using the binaries
    
5.  Start Kafka using the binaries in another process
    
6.  Setup the $PATH environment variables for easy access to the Kafka binaries
    

### Installing Java JDK 11

[](#Installing-Java-JDK-11-0)

To install Apache Kafka on Mac, Java 11 is the only prerequisite.

1.  Navigate to [Amazon Corretto 11 download page](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html) (the Amazon distribution of OpenJDK 11) install [amazon-corretto-11-x64-macos-jdk.pkg](https://corretto.aws/downloads/latest/amazon-corretto-11-x64-macos-jdk.pkg)
    
2.  Double click on the downloaded file and proceed with the installation steps.
    
3.  You have successfully installed Java JDK 11. You may delete the .pkg file.
    

![Screenshot of Amazon Corretto installer that will give you the distribution of OpenJDK 11 needed to install Apache Kafka on Mac](../static/images/image__28_.webp "Amazon Corretto Installer - the Amazon distribution of OpenJDK 11")

Please follow the instructions [here](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/macos-install.html#macos-install-instruct) to verify your installation of Amazon Corretto 11. Instructions on how to set the default version of Java on a Mac can be found [here](https://stackoverflow.com/a/24657630).

Upon completion, you should get a similar output when doing `java -version`:

```
openjdk version "11.0.10" 2021-01-19 LTS
OpenJDK Runtime Environment Corretto-11.0.10.9.1 (build 11.0.10+9-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.10.9.1 (build 11.0.10+9-LTS, mixed mode)shell
```

### Install Apache Kafka on Mac OS X

[](#Install-Apache-Kafka-on-Mac-OS-X-1)

1\. Download the latest version of Apache Kafka from [https://kafka.apache.org/downloads](https://kafka.apache.org/downloads) under Binary downloads.

![The download page for Apache Kafka where you can download and install Kafka.](../static/images/image__29_.webp "Install Kafka - Apache Kafka Download")

2\. Click on any of the binary downloads (it is preferred to choose the most recent Scala version - example 2.13). For this illustration, we will assume version `2.13-3.0.0`.

3\. Download and extract the contents (double click in the Finder) to a directory of your choice, for example `~/kafka_2.13-3.0.0` .

4\. Navigate to the root of the Apache Kafka folder and open a Terminal. Or Open a Terminal and navigate to the root directory of Apache Kafka. For this example, we will assume that the Kafka download is expanded into the `~/kafka_2.13-3.0.0` directory.

**Start Zookeeper**

Apache Kafka depends on Zookeeper for cluster management. Hence, prior to starting Kafka, Zookeeper has to be started. There is no need to explicitly install Zookeeper, as it comes included with Apache Kafka.

Make sure your `JAVA_HOME` environment variable is set first, as instructed in the Install Java section, so that Java 11 is used when doing `java -version`

From the root of Apache Kafka, run the following command to start Zookeeper:

```
~/kafka_2.13-3.0.0/bin/zookeeper-server-start.sh ~/kafka_2.13-3.0.0/config/zookeeper.properties
```

![A screenshot of the process for starting Zookeeper for Apache Kafka on Mac OS.](../static/images/Screen_Shot_2022-01-06_at_18.webp "Starting Zookeeper for Apache Kafka on Mac")

> [!TIP]
> **Daemon Mode**
>
> You can use the `-daemon` flag to run Zookeeper in daemon mode in the background

#### **Start Apache Kafka**

**Open another Terminal window** and run the following command from the root of Apache Kafka to start Apache Kafka.



```
~/kafka_2.13-3.0.0/bin/kafka-server-start.sh ~/kafka_2.13-3.0.0/config/server.properties
```

![A terminal screenshot showing that Apache Kafka is now running on Mac.](../static/images/Screen_Shot_2022-01-06_at_18.webp "Starting Apache Kafka on Mac via Terminal")

Ensure to keep both terminal windows opened, otherwise you will shut down Kafka or Zookeeper.

**Kafka is now started, congratulations!**

### Setup the $PATH environment variable

[](#Setup-the-$PATH-environment-variable-2)

In order to easily access the Kafka binaries, you can edit your PATH variable by adding the following line (edit the content to your system) to your system run commands (for example `~/.zshrc` if you use zshrc):

`PATH="$PATH:/Users/stephanemaarek/kafka_2.13-3.0.0/bin"`

This ensures that you can now run the kafka commands without prefixing them.

After reloading your terminal, the following should work from any directory

```
kafka-topics.sh
```

### Optional: Changing the Kafka and Zookeeper data storage directory

[](#Optional:-Changing-the-Kafka-and-Zookeeper-data-storage-directory-3)

For Zookeeper:

*   edit the `zookeeper.properties` file at `~/kafka_2.13-3.0.0/config/zookeeper.properties` and set the following to your heart's desire `dataDir=/your/path/to/data/zookeeper`
    
*   start Zookeeper using the update `zookeeper.properties` file as shown above
    
*   you can also make a copy of the `zookeeper.properties` file anywhere in your computer and edit that file instead, and reference it in the Zookeeper start command shown above
    

For Kafka:

*   edit the `server.properties` file at `~/kafka_2.13-3.0.0/config/server.properties` and set the following to your heart's desire `log.dirs=/your/path/to/data/kafka`
    
*   start Zookeeper using the update `server.properties` file as shown above
    
*   you can also make a copy of the `server.properties` file anywhere in your computer and edit that file instead, and reference it in the Zookeeper start command shown above