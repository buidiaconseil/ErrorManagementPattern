# Error handling in data flows
## Main design points

When an error arrives, a number of solutions are possible. From ignorance to automatic correction, the following paragraphs describe some of these possibilities.

### Ignore the error
We can ignore the error, but this must be motivated by the pure and simple acceptance of abnormal treatment on this data.
### Isolate the error
Isolating the error is certainly one of the most suitable strategies for reactive and efficient flow management. The isolation itself poses the problem of backup and subsequent processing of the untreated message. This subject will be dealt with a little later.
### Repeat the message
It's about taking the message and replaying it. One tries to replay a number of times the message hoping that the error is due to an unavailability or an incompatible scheduling problem. A maximum restart count must be used to avoid any infinite loop. If resolution is impossible then we turn to the two previous solutions (ignore or isolate).
### Repeat the message with a delay.
Like the previous paragraph, it is a question of starting the treatment again. But this time, we set a delay between each treatment. There are two types of delay: the linear delay or the so-called exponential delay. The latter increases the delay gradually according to the number of restart.
### Automatic correction of messages
This solution although very complex is a solution considered when the data content is massive. The big idea is to have an education of the corrections of errors made. Once this learning done, we try to predict the correct correction with the previous algorithm. Of course this can only be done on mastered data. For example: An unspecified field (Ex: City, ...) can be replaced by the same field based on similar data previously learned thanks to a common information between the messages. (Ex: Number of a Component).
When the message is corrected, we return to previous solutions.

## Design choice
During the review of the flows managed by Kafka, it was identified the need for an equivalent to the JMS functions of error message management. A use of specialized flows must allow a management of reminders and equivalent errors.

The main idea is to never stop the flow of errors and relay this error management to other flows.

The system is currently designed to replay messages. There will be a management flow of the Reminders. But when it is impossible to replay messages, we also need an error flow.

The solution can be put in state diagram. The steps identified are: Detection, Error, Replay, Wait and Finished

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/ErrorStateManagement.wsd)

We always start with a state of detection of the error. If there is no error, then the processing status is over. If it is in error the state becomes in error. At this stage, the error is not solvable. Either the error is restartable. In this case, either we wait for a specified time in advance, or we start again directly. It will be necessary to define the conditions of insolvency in another article.

## A solution with two streams
There are two streams of data to handle errors on the one hand and replay on the other.
• The error flow associated with a database. The database is responsible for handling the processing status of the error.
• The replay flow requires a precise adjustment of the waiting times.

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/errorFlow.wsd)

When an error is detected, the flow is cut in half. One part for the flows in errors and another for the streams in replays.

The wait for a message in the stream is made at the moment of consumption. We check the date and time plus the delay. If the time is greater than the current date, the message is pushed to the source stream by adding the replay information.
When a message has already been identified as a replay, the number of trials is incremented. If the number of feasible tests is reached, we stop the replay and push into the error stream.

The error queue is part of a particular treatment. Indeed, each new message in error is immediately consumed for creation of a recording in the database. This record must be consumed by an operator via a suitable user interface.

When the processing of the message is finished, either by the resolution of the problem, or by the abandonment pure and simple, one deletes this recording. *
The operator is responsible for handling messages in error and tracking the progress of replay.

## Message Replay Feed
It is an intermediate flow of backup of the states of replays of messages in errors.

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/ExplodePartitionByTime.wsd)

Whenever a message has to be replayed, it is pushed into this stream.

In Kafka, each message must report a consumption commit per partition. The main idea is to use this feature to manage different time conditions of stream replay.

The consumption of this stream is made by partition. Each partition corresponds to a different but ordered replay delay.

Each message is time stamped and lets you know when it should be replayed. As the messages are ordered, just consume them sequentially by partition.

The choice of consumption can be made within the same consumption group. It is thus easier to add new customers within the same group who will consume different partitions and at the same time different replay times.

This strategy implies that there must be more score than number of Replay. Because if this is not the case, a partition will be able to host several Rejoue at different times. This can cause irregular wait times with late replay.

## Simple Consumption Algorithm

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/RetryFlow.wsd)

The algorithm is simple to understand. Each record is on a score so a different replay. Just do not commit on the record that is not yet to be processed.

Each "poll" retrieves a "compact" version of the partitions. Thus, there is only one record per score present in the list of Records.

A more complex algorithm exists and consists in using all the available records and then solving the good "Offset" of commit per partition.

## Manual Error Resolution
In the event of an unrecoverable error, their management imposes a strict follow-up. No messages should be discarded without manual verification.

Of course, this verification can be massive by grouping identical error messages.

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/errorFinal.wsd)

Once the message has been pushed into the error girl, it is detected by a process responsible for initializing the records related to error handling.

The operator, who has been notified, lists the errors. When he wants the content of the message, he goes to the stream and retrieves it with his offset number. After processing, he can choose not to change the status of the error message. He can also replay the message. 

There is little, when the conditions are acceptable delete the registration of the error in the database. This helps to ensure a healthy database, containing only information not yet processed.
