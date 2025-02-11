How to install Apache Kafka on Mac with Homebrew
================================================

Learn how to start Apache Kafka on Mac OS with Homebrew (brew)

* * *

How to Install Kafka with Homebrew?
-----------------------------------

[](#How-to-Install-Kafka-with-Homebrew?-0)

1.  Install Homebrew
    
2.  Run `brew install kafka`
    
3.  Start Zookeeper using the CLI
    
4.  Start Kafka using the CLI in another terminal
    

> [!TIP]
> **Control vs Homebrew**
>
> For more control over your Kafka installation and different binary versions, we prefer using the method above instead of Homebrew. Nonetheless, here are the steps to install Kafka using Homebrew

If you have already installed Homebrew for Mac OS, you can use it to install Kafka in one step. This will ensure that you have Java installed first, and it will then install Apache Kafka.

If you have not yet installed Homebrew, do that first by following the directions on the [installation page](https://brew.sh/).

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then you can install Kafka itself. The Homebrew package manager will ensure that you have all the dependencies installed first, including the necessary Java version:

```
brew install kafka
```

Homebrew's default installation path will differ based on the chip: Macs using Intel will install Kafka under `/usr/local/Cellar.` Macs with Apple silicon will install Kafka under `opt/homebrew/Cellar`. In both cases, the files will be linked into other directories:

*   Binaries and scripts will be in `/usr/local/bin` or `opt/homebrew/bin`
    
*   Kafka configurations will be in `/usr/local/etc/kafka` or `opt/homebrew/etc/kafka`
    
*   Zookeeper configuration will be in `/usr/local/etc/zookeeper` or `opt/homebrew/etc/zookeeper`
    
*   The `log.dirs` config (the location for Kafka data) will be set to `/usr/local/var/lib/kafka-logs` or `opt/homebrew/var/lib/kafka-logs`
    

> [!WARNING]
> **Macs with Apple Chips**
>
> Remember, if you have a Mac with an M chip from Apple, you will need to replace `/usr/local/` with `/opt/homebrew/`. The instructions are identical otherwise.

#### **Start Zookeeper**

Apache Kafka depends on Zookeeper for cluster management. Hence, prior to starting Kafka, Zookeeper has to be started. There is no need to explicitly install Zookeeper, as it comes included with Apache Kafka.

After installation is complete, you can start Zookeeper using the following command


```
/usr/local/bin/zookeeper-server-start /usr/local/etc/zookeeper/zoo.cfg
```

![Terminal screenshot showing that Zookeeper for Kafka is running.](../static/images/Screen_Shot_2022-01-06_at_18.webp "Starting Zookeeper for Kafka via Homebrew on Mac OS")

#### **Start Apache Kafka**

**Open another Terminal window** and run the following command from the root of Apache Kafka to start Apache Kafka.

[](javascript:void(0);)

```
/usr/local/bin/kafka-server-start /usr/local/etc/kafka/server.properties
```

![Terminal screenshot taken when starting Apache Kafka via Homebrew on Mac](../static/images/Screen_Shot_2022-01-06_at_18.webp "Starting Kafka via Homebrew for Mac")

Ensure to keep both terminal windows opened, otherwise you will shut down Kafka or Zookeeper.

**Kafka is now started, congratulations!**

* * *

#### **Optional: Changing the Kafka and Zookeeper data storage directory when using Homebrew**

For Zookeeper:

*   edit the `zoo.cfg` file at `/usr/local/etc/zookeeper/zoo.cfg` and set the following to your heart's desire `dataDir=/your/path/to/data/zookeeper`
    
*   start Zookeeper using the update `zoo.cfg` file as shown above
    
*   you can also make a copy of the `zoo.cfg` file anywhere in your computer and edit that file instead, and reference it in the Zookeeper start command shown above
    

For Kafka:

*   edit the `server.properties` file at `/usr/local/etc/kafka/server.properties` and set the following to your heart's desire `log.dirs=/your/path/to/data/kafka`
    
*   start Zookeeper using the update `server.properties` file as shown above
    
*   you can also make a copy of the `server.properties` file anywhere in your computer and edit that file instead, and reference it in the Zookeeper start command shown above.

---
Next: [How to Install Apache Kafka on Linux](https://github.com/AbdoMusk/Apache-Kafka/blob/main/2-%20Starting%20Kafka/5-%20How%20to%20Install%20Apache%20Kafka%20on%20Linux.md)