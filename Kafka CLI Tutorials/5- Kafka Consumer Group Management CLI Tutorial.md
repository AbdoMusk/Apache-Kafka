Kafka Consumer Group Management CLI Tutorial
============================================

Learn to work with consumer groups in Kafka using CLI

* * *

The Kafka Consumer Groups CLI kafka-consumer-groups is used to manage consumer groups in Kafka. Make sure you have [started Kafka](/kafka/starting-kafka/) beforehand.

> [!TIP]
> **CLI Extensions**
>
> Use CLI commands with appropriate extensions for your platform, e.g., `kafka-consumer-groups.bat` for windows, `kafka-consumer-groups.sh` for Linux

* * *

How to reset a Kafka consumer group using the CLI?
--------------------------------------------------

[](#How-to-reset-a-Kafka-consumer-group-using-the-CLI?-0)

To reset a Kafka consumer groups, we need to:

*   Find your broker hostname and port e.g., `localhost:9092`
    
*   Understand the offset reset strategy (to earliest, to latest, to specific offset, shift by...)
    
*   Stop the running consumer groups (otherwise the command will fail)
    
*   Use the `kafka-consumer-groups.sh` CLI with the `--reset-offsets` option
    

Example: Reset offsets to the earliest
--------------------------------------

[](#Example:-Reset-offsets-to-the-earliest-1)

First, ensure that the consumers are stopped ("has no active members")

```
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application

Consumer group 'my-first-application' has no active members.
```

Observe the current offsets for your consumer group (same command as above)

```
GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
my-first-application first_topic     0          3               3               0               -               -               -
my-first-application first_topic     1          5               5               0               -               -               -
my-first-application first_topic     2          6               6               0               -               -               -
```

We will reset the offsets to the earliest position in order to read the topic entirely again

```
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --topic first_topic

GROUP                          TOPIC                          PARTITION  NEW-OFFSET
my-first-application           first_topic                    0          0
my-first-application           first_topic                    1          0
my-first-application           first_topic                    2          0
```

As you can see the new offsets for that consumer group for all partitions are 0, which means that upon restarting a consumer in that group, it will read from the beginning of each partition:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application
third message
fifth message
seventh message
tenth message
first message
fourth message
eigth message
hello
world
second message
sixth message
ninth message
```

**Note that messages are read in order for each partition, not across partitions (we have 3 partitions in this example) - we will never stop reminding you this.**

Example: Reset offsets shift by
-------------------------------

[](#Example:-Reset-offsets-shift-by-2)

Stop the running consumers to be able to reset offsets.

Shift by allows you to rewind offsets by a specific value (negative to go back in messages and positive to advance in messages).

```
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application

Consumer group 'my-first-application' has no active members.
GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
my-first-application first_topic     0          3               3               0               -               -               -
my-first-application first_topic     1          5               5               0               -               -               -
my-first-application first_topic     2          6               6               0               -               -               -
```

In this example we reset offsets by shifting by `-2` for the consumer group `my-first-application` subscribed on the topic `first_topic`

```
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --execute --topic first_topic

GROUP                          TOPIC                          PARTITION  NEW-OFFSET
my-first-application           first_topic                    0          1
my-first-application           first_topic                    1          3
my-first-application           first_topic                    2          4
```

As you can see the offsets decreased by 2 for each partition.

Read messages from the topic first\_topic using Kafka console consumer CLI. It will only return last 2 messages from each partition of the topic.

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application
seventh message
tenth message
fourth message
eigth message
sixth message
ninth message
```

### Gotchas

[](#Gotchas-0)

Here are the common mistakes and caveats with the `kafka-consumer-groups.sh` command:

*   **You cannot reset a consumer group if consumers are active in it.**
    
*   This command can be used to reprocess data for a consumer group (in case you have a bug fix)
    
*   This command be also be used to advance message consumption in Kafka (for example if a message is a poison pill, or if your consumer is too slow to catch up with the entire topic).
    

Extra: Important options you can set (advanced)
-----------------------------------------------

[](#Extra:-Important-options-you-can-set-(advanced)-3)

`--all-groups`

Applies to all groups, use with caution

`--all-topics`

Consider all topics assigned to a group in the reset-offsets process, use with caution

`--by-duration`

Reset to offsets by duration

`--dry-run`

Only show the expected result, but does not actually run the command

`--to-datetime`, `--by-period`, `--to-earliest`, `--to-latest`, `--shift-by`, `--from-file`, `--to-current`

All the various options available to you to reset the offsets

* * *

How to list all Kafka consumers in a consumer groups using the CLI?
-------------------------------------------------------------------

[](#How-to-list-all-Kafka-consumers-in-a-consumer-groups-using-the-CLI?-4)

Listing all the Kafka consumer in a consumer group help you understand where the consumers are placed on your network, and how far they are into the topic consumption.

```
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application

GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                                          HOST            CLIENT-ID
my-first-application first_topic     0          3               3               0               consumer-my-first-application-1-0237b0a1-911d-45f1-891f-8fd7630a7593 /172.19.0.1     consumer-my-first-application-1
my-first-application first_topic     1          5               5               0               consumer-my-first-application-1-70ddc756-8dbc-45e4-b5b6-1e5a75db9e62 /172.19.0.1     consumer-my-first-application-1
my-first-application first_topic     2          6               6               0               consumer-my-first-application-1-a8ce2af3-97b3-4445-bba3-f4a5f6c4464d /172.19.0.1     consumer-my-first-application-1
```

The `CONSUMER ID` represents the unique identifier of the consumer to the Kafka broker

The `CLIENT ID` represents a client-side setting that you can optionally set to identify a consumer in your consumer groups (with the `client.id` consumer property)

The `CURRENT-OFFSET` is the latest committed offset for that group

The `LOG-END-OFFSET` represents the latest message offset available in the topic-partition for consumption

The `LAG` is the difference of `LOG-END-OFFSET` and `CURRENT-OFFSET` and represents how far behind a consumer is to the tail of a topic.

The `HOST` is the hostname / IP of the consumer client machine.

* * *

How to List of all Kafka consumers groups using the CLI?
--------------------------------------------------------

[](#How-to-List-of-all-Kafka-consumers-groups-using-the-CLI?--5)

Listing the consumer groups help you understand which ones could be down, or not stable.

**Listing consumer groups state**

```
kafka-consumer-groups --bootstrap-server localhost:9092 --list --state
GROUP                STATE
my-first-application Stable
```

**Describe all consumer groups and state** (helpful for assignment strategy and coordinator ID)

```
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --all-groups  --state

GROUP                     COORDINATOR (ID)          ASSIGNMENT-STRATEGY  STATE           #MEMBERS
my-first-application      127.0.0.1:9092 (1)        range                Stable          3
```

You can instead of using `--all-groups` just specify the command for one group only.

> [!TIP]
> **Note:**
>
> As of Kafka >= 2.1, the "console-consumer-" groups will not appear in the output

* * *

How to delete a consumer group in Kafka using the CLI?
------------------------------------------------------

[](#How-to-delete-a-consumer-group-in-Kafka-using-the-CLI?-6)

You may want to delete a consumer group in order to reset entirely the reading mechanism. For this, you can use the `delete` option:

```
$ kafka-consumer-groups --bootstrap-server localhost:9092 --delete --group my-first-application
Deletion of requested consumer groups ('my-first-application') was successful.
```

Alternatively, if you want to only delete offsets for a specific topic (helpful when your consumer group is reading from multiple topics) you can use the following command:

```
kafka-consumer-groups --bootstrap-server localhost:9092 --delete-offsets --group my-first-application --topic first_topic
Request succeed for deleting offsets with topic first_topic group my-first-application

TOPIC                          PARTITION       STATUS
first_topic                    0               Successful
first_topic                    1               Successful
first_topic                    2               Successful
```
