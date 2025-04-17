1. **crtmqm** - Create a queue manager.
   ```bash
   crtmqm QM1
   ```
   This command creates a queue manager named `QM1`.

2. **crtmqm -q host1/qm1** - Create a queue manager with the default configuration and set it as the default queue manager (-q) on this computer.
   ```bash
   crtmqm -q host1/qm1
   ```
   This command creates a queue manager named `host1/qm1` and sets it as the default queue manager on the computer.

3. **dspmq** - Check the creation of the queue manager.
   ```bash
   dspmq
   ```
   This command displays the status of all queue managers on the system, allowing you to verify the creation of `QM1`.

4. **amqmdain/strmqm** - Start the default queue manager.
   ```bash
   amqmdain qmgr start QM1
   strmqm QM1
   ```
   These commands start the queue manager `QM1`. The first command is used in Windows, and the second is used in Linux.

5. **amqmdain qmgr start** - Command to start the queue manager so that it does not stop when logging out and logging back in (Windows).
   ```bash
   amqmdain qmgr start QM1
   ```
   This command ensures that the queue manager `QM1` continues running even after logging out and logging back in on a Windows system.

6. **strmqm** - Command to start the queue manager so that it does not stop when logging out and logging back in (Linux).
   ```bash
   strmqm QM1
   ```
   This command ensures that the queue manager `QM1` continues running even after logging out and logging back in on a Linux system.

7. **vi /var/mqm/mqs.ini >> DefaultQueueManager: \n Name=host1/qm1** - Make the current message manager the default message manager.
   ```bash
   vi /var/mqm/mqs.ini
   ```
   Add the following lines to the `mqs.ini` file:
   ```
   DefaultQueueManager:
   Name=host1/qm1
   ```
   This sets `host1/qm1` as the default queue manager in the configuration file.

Sure, here are the commands translated to English with examples:

### Creating a Local Queue:
8. **runmqsc** - Start an interactive MQSC session with the default queue manager.
   ```bash
   runmqsc
   ```

9. **DEFINE QLOCAL('queue1') + DESCR('Redbook example queue1: Newly defined')** - Command to create a queue.
   ```bash
   DEFINE QLOCAL('queue1') + DESCR('Redbook example queue1: Newly defined')
   ```
   This command creates a local queue named `queue1` with a description.

10. **DEFINE QLOCAL('queue1') REPLACE + DESCR('Redbook example queue1: Newly defined')** - Command to create (replace) a queue.
   ```bash
   DEFINE QLOCAL('queue1') REPLACE + DESCR('Redbook example queue1: Newly defined')
   ```
   This command creates or replaces the local queue named `queue1` with a description.

11. **END** - Exit the interactive MQSC session.
   ```bash
   END
   ```

### Displaying Attributes of the New Queue:
12. **runmqsc** - Start an interactive MQSC session with the default queue manager.
   ```bash
   runmqsc
   ```

13. **DISPLAY QLOCAL(*)** - Display a list of local queue objects.
   ```bash
   DISPLAY QLOCAL(*)
   ```

14. **DISPLAY QLOCAL('queue1') ALL** - View all attributes of the specified queue.
   ```bash
   DISPLAY QLOCAL('queue1') ALL
   ```

15. **DISPLAY QLOCAL('queue1') DEFPSIST DESCR CURDEPTH** - Display attributes such as default persistence, description, and current depth of the queue.
   ```bash
   DISPLAY QLOCAL('queue1') DEFPSIST DESCR CURDEPTH
   ```

16. **DISPLAY QUEUE(*)** - Display all queue objects defined in the queue manager.
   ```bash
   DISPLAY QUEUE(*)
   ```

17. **END** - Exit the interactive MQSC session.
   ```bash
   END
   ```

### Modifying Queue Object Attributes:
18. **runmqsc** - Start an interactive MQSC session with the default queue manager.
   ```bash
   runmqsc
   ```

19. **ALTER QLOCAL('queue1') + DESCR('Redbook example queue1: Altered description')** - Change the queue description attribute.
   ```bash
   ALTER QLOCAL('queue1') + DESCR('Redbook example queue1: Altered description')
   ```

20. **DISPLAY QLOCAL('queue1') DESCR** - Display the updated queue attribute.
   ```bash
   DISPLAY QLOCAL('queue1') DESCR
   ```

21. **END** - Exit the interactive MQSC session.
   ```bash
   END
   ```

### Adding Test Messages to the Queue:
**Note:** The path to the directory with sample programs for WebSphere MQ must be added to the search path in the OS environment variables.
22. **amqsput queue1** - Add test messages to the queue.
   ```bash
   amqsput queue1
   ```
   Sample output:
   ```
   Sample AMQSPUT0 start
   target queue is queue1
   ```

### Viewing Messages Added to the Queue (Without Deleting Them):
23. **amqsbcg queue1 > queue1.txt** - View queue messages and output to `queue1.txt`.
   ```bash
   amqsbcg queue1 > queue1.txt
   ```

   Sample output:
   ```
   # MsgType : 8 - Type 8 corresponds to datagrams. Requests are type 1, responses are type 2, reports are type 4.
   # Persistence : 0 - Value "0" indicates the message is non-persistent, while value "1" indicates the message is persistent.
   # ReplyToQ : ' ' - This field displays the 48-character name of the reply-to queue. If the name is less than 48 characters, the missing characters are replaced with spaces on the right. Such data is called blank-padded and is often used in WebSphere MQ structures. In this example, this field in the message descriptor is empty.
   # ReplyToQMgr : 'host1/qm1 ' - This field contains the 48-character blank-padded name of the reply-to queue manager, automatically filled by the queue manager when placing the message.
   ```
Message Content
Length: 20 bytes

Binary and Text Representation:

00000000: 5265 6462 6F6F 6B20 7465 7374 206D 6573   'Redbook test mes'
00000010: 7361 6765                     'sage '
The message content is displayed in both its binary form (on the left) and its text representation (on the right).

Defining and Using Local Queue Aliases
Start an interactive MQSC session:

   ```bash
runmqsc
   ```
Define a queue alias for persistent messages:

   ```bash
DEFINE QALIAS('queue1.persistent') TARGQ('queue1') DEFPSIST(YES) +
DESCR('Redbook example alias to queue1: For persistent messages')
   ```
Example: Suppose you need a queue alias for an application sending guaranteed persistent messages. You can define queue1.persistent pointing to queue1.

Check the attributes of the queue alias:

   ```bash
DISPLAY QALIAS('queue1.persistent') TARGQ DEFPSIST DESCR
   ```
Example Output:

QALIAS(queue1.persistent) TARGQ(queue1) DEFPSIST(YES) DESCR('Redbook example alias to queue1')
Send test messages via the alias using the sample program:

   ```bash
amqsput queue1.persistent
   ```
Example: Running this command will prompt message input; an empty line exits the program.

Stopping and Restarting the Queue Manager
Graceful shutdown of the queue manager:

   ```bash
endmqm -w host1/qm1
   ```
Immediate shutdown:

   ```bash
endmqm -i host1/qm1
   ```
Restart on Windows:

   ```bash
amqmdain qmgr start
   ```
Restart on UNIX:

   ```bash
strmqm
   ```
Example: If your queue manager qm1 encounters a failure, you may use strmqm to restart it on UNIX.

Retrieving Messages from a Queue (Messages are Deleted)
Extract messages from the default queue manager:

   ```bash
amqsget queue1
   ```
Example: Running this command retrieves all messages from queue1 and removes them.

Deleting a Queue Alias
Start an interactive MQSC session:

   ```bash
runmqsc
   ```
Delete a queue alias object:

   ```bash
DELETE QALIAS('queue1.persistent')
   ```
Example Output: After deletion, the alias will no longer appear in:

   ```bash
DISPLAY QUEUE(*)
DISPLAY QALIAS(*)
   ```
Creating an Alias for a Queue Manager Using a Remote Queue Object
Define an alias for remote queue manager host1/qm1:

   ```bash
DEFINE QREMOTE('host1/qm1.alias') RNAME('') RQMNAME('host1/qm1') +
DESCR('Redbook example queue manager alias to host1/qm1')
   ```
Example: If applications need a logical alias instead of the actual queue manager name, use host1/qm1.alias to refer to host1/qm1.

Check attributes of the remote queue alias:

   ```bash
DISPLAY QREMOTE('host1/qm1.alias') RNAME RQMNAME DESCR
   ```
To add multiple messages to a queue, use:

   ```bash
amqsput queue1 host1/qm1 8208 0 host1/qm1.alias
   ```
Parameters explained:

queue1 – Queue name.

host1/qm1 – The queue manager name to connect to for message sending.

8208 – Decimal code for request parameters passed during MQOPEN. This opens the message queue for writing. If the queue manager is shutting down, further attempts fail.

0 – Code defining MQCLOSE without parameters (not relevant here).

host1/qm1.alias – Queue manager alias used in MQOPEN. Instead of referring directly to the queue manager, it uses a previously defined alias, which is resolved to host1/qm1.

Example: If a queue queue1 is hosted on host1/qm1, using an alias like host1/qm1.alias ensures applications remain flexible without hardcoding direct connections.

Stopping and Deleting a Queue Manager
Graceful shutdown:

   ```bash
endmqm -w host1/qm1
   ```
Immediate shutdown:

   ```bash
endmqm -i host1/qm1
   ```
Permanent deletion of the queue manager:

   ```bash
dltmqm host1/qm1
   ```
Creating and Starting a Queue Manager for a Service
Create a queue manager called host1/echo.hub:

   ```bash
crtmqm host1/echo.hub
   ```
Start the queue manager:

Windows:

   ```bash
amqmdain qmgr start host1/echo.hub
   ```
Unix/Linux:

   ```bash
strmqm host1/echo.hub
   ```
Create a local queue to host the echo service:

   ```bash
DEFINE QLOCAL('echo') + DESCR('Queue hosting the echo service')
   ```
Example: This queue will hold requests and return responses with the same content.

Manually Defining a Reply Queue
Start an interactive MQSC session:

   ```bash
runmqsc host1/echo.hub
   ```
Create a manually defined reply queue:

   ```bash
DEFINE QLOCAL('echo.replies.manual') + DESCR('Manually defined reply-to queue for echo service')
END
   ```
Example: Applications sending requests can use echo.replies.manual as the reply queue to receive responses.

Sending and Inspecting a Test Request
   ```bash
amqsreq echo host1/echo.hub echo.replies.manual
   ```
Parameters:

echo – Service queue (no active services processing yet).

host1/echo.hub – Queue manager to connect to.

echo.replies.manual – The reply queue name.

Triggering a Service When Messages Arrive
Set up an initiation queue:

   ```bash
DEFINE QLOCAL('echo.initq') + DESCR('Initiation queue for triggering the echo service')
   ```
Activate first-message triggering:

   ```bash
ALTER QLOCAL('echo') + TRIGGER TRIGTYPE(FIRST) INITQ('echo.initq') + PROCESS('amqsech')
   ```
Start the WebSphere MQ trigger monitor:

   ```bash
runmqtrm -m host1/echo.hub -q echo.initq
   ```
Example: When a message arrives in echo, a trigger message is generated in echo.initq, starting the process amqsech.

Sending an Echo Request via a Temporary Dynamic Queue
Create a model queue for dynamic replies:

   ```bash
DEFINE QMODEL('echo.replies.tempdyn') + DESCR('Temporary dynamic reply-to queues for echo service')
   ```
Send a request:

   ```bash
amqsreq echo host1/echo.hub echo.replies.tempdyn
   ```
Check dynamically created queues:

   ```bash
DISPLAY QL('AMQ.*') DEFTYPE DESCR
   ```
Example: This method automatically generates temporary queues for receiving responses, optimizing queue management.

Running a Broker on the Queue Manager
Enable and start the broker service:

   ```bash
ALTER SERVICE('SYSTEM.BROKER') CONTROL(QMGR)
START SERVICE('SYSTEM.BROKER')
   ```
Example: This allows message processing automation.

Setting Up a Listener and a Server Connection Channel
Define and start a TCP listener:

   ```bash
DEFINE LISTENER('LISTENER.TCP') + TRPTYPE(TCP) PORT(9001) CONTROL(QMGR) +
DESCR('TCP/IP Listener for queue manager')
START LISTENER('LISTENER.TCP')
   ```
Create a server connection channel:

   ```bash
DEFINE CHANNEL('all.clients') CHLTYPE(SVRCONN) MCAUSER('username')
   ```
Example: Applications connect using this channel.

Sending Messages Using Environment Variables
Set up the MQSERVER variable:

   ```bash
MQSERVER=all.clients/TCP/'host1.example.com(9001)'
export MQSERVER
   ```
Send messages:

   ```bash
amqsputc queue1 host1/echo.hub
   ```
Retrieve messages:

   ```bash
amqsgetc queue1 host1/echo.hub
   ```
Example: MQSERVER simplifies queue manager connectivity.

Setting Up a Dead Letter Queue
Create and assign a dead-letter queue:

   ```bash
DEFINE QLOCAL('dead.letters')
ALTER QMGR DEADQ('dead.letters')
   ```
Example: Unprocessed messages are redirected to dead.letters.

Configuring Message Channels
Create a sender-receiver channel:

   ```bash
DEFINE CHANNEL('to.host1/echo.hub') CHLTYPE(RCVR)
DEFINE CHANNEL('to.host1/echo.hub') CHLTYPE(SDR) +
CONNAME('host1.example.com(9001)') XMITQ('host1/echo.hub')
   ```
Ping the channel:

   ```bash
PING CHANNEL('to.host1/echo.hub')
   ```
Example: If connectivity fails, check hostnames, ports, and listener status.

Triggering a Communication Channel
Set up automatic triggering:

   ```bash
ALTER QLOCAL('host1/echo.hub') TRIGGER TRIGTYPE(FIRST) +
TRIGDATA('to.host1/echo.hub') INITQ('SYSTEM.CHANNEL.INITQ')
   ```
Send a test message to another queue manager:

   ```bash
amqsput queue1 host2/spoke 8208 0 host1/echo.hub
   ```
Example: Messages flow from one queue manager to another dynamically.
Sending a Message to a Queue Using a Remote Queue Manager
To add messages to a queue using a remote queue manager:

   ```bash
amqsput queue1 host1/echo.hub 8208 0 host2/spoke
   ```
Parameters explained:

queue1 – The queue name within host1/echo.hub.

host2/spoke – The queue manager that needs to be connected.

8208 – Decimal code passed during MQOPEN (forces the queue to be opened for writing; fails if the queue manager is shutting down).

0 – Defines MQCLOSE without parameters.

host1/echo.hub – Queue manager name used in MQOPEN, corresponding to the remote queue manager and transmission queue used by host2/spoke.

Check the channel status:

   ```bash
DISPLAY CHSTATUS('to.host1/echo.hub')
   ```
Start the sender channel manually:

   ```bash
START CHANNEL('to.host1/echo.hub')
   ```
Stop the channel without blocking it:

   ```bash
STOP CHANNEL('to.host1/echo.hub') MODE(INACTIVE)
   ```
Creating a Receiver Channel for a Peripheral Queue Manager
Define a receiver channel:

   ```bash
DEFINE CHANNEL('to.host2/spoke') CHLTYPE(RCVR)
   ```
Creating a Transmission Queue for a Central Queue Manager
Define a transmission queue that will send messages to host2/spoke:

   ```bash
DEFINE QLOCAL('host2/spoke') USAGE(XMITQ) +
TRIGGER TRIGTYPE(FIRST) +
TRIGDATA('to.host2/spoke') INITQ('SYSTEM.CHANNEL.INITQ') +
DESCR('Transmission queue for messages to host2/spoke')
Defining a Sender Channel for a Central Queue Manager
   ```
   ```bash
DEFINE CHANNEL('to.host2/spoke') CHLTYPE(SDR) +
CONNAME('host1.example.com(9002)') XMITQ('host2/spoke')
   ```
Defining a Remote Queue Locally
A local definition of a remote queue:

   ```bash
DEFINE QREMOTE('echo') RNAME('echo') RQMNAME('host1/echo.hub') +
DESCR('Local definition for routing requests for the echo service')
   ```
This allows requests to be routed via the local manager.

Defining a Reply Queue for a Peripheral Queue Manager
   ```bash
DEFINE QMODEL('echo.replies')
   ```
Send a request to the echo service via the peripheral queue manager:

   ```bash
amqsreq echo host2/spoke echo.replies
   ```
Assigning Full Repository Queue Managers for Clustering

   ```bash
ALTER QMGR REPOS('example.cluster')
   ```
Defines a queue manager repository within the cluster.

Defining Clustered Receiver and Sender Channels
Receiver channel:

   ```bash
DEFINE CHANNEL('clus.host1/full') CHLTYPE(CLUSRCVR) +
CONNAME('host1.example.com(9031)') CLUSTER('example.cluster')
   ```
Sender channel:

   ```bash
DEFINE CHANNEL('clus.host2/full') CHLTYPE(CLUSSDR) +
CONNAME('host2.example.com(9033)') CLUSTER('example.cluster')
   ```
Sharing Access to Cluster Queues
Define a queue shared across the cluster:

   ```bash
DEFINE QLOCAL('cluster.queue') CLUSTER('example.cluster')
   ```
Enable load balancing:

   ```bash
ALTER QMGR CLWLUSEQ(ANY)
   ```
Example: Load balancing can be tested by sending multiple messages to the clustered queue:

   ```bash
i=0; while [ $i -lt 100 ]; do let i=i+1; echo Message$i | amqsput cluster.queue host1/partial; done
(Unix/Linux)

   ```bash
FOR /L %1 IN (1,1,100) DO echo Message%1 | amqsput cluster.queue host1/partial
   ```
(Windows)

Publishing the Echo Service to the Cluster
Define the clustered receiver channel:

   ```bash
DEFINE CHANNEL('clus.host1/echo.hub') CHLTYPE(CLUSRCVR) +
CONNAME('host1.example.com(9001)') CLUSTER('example.cluster')
   ```
Define the clustered sender channel:

   ```bash
DEFINE CHANNEL('clus.host1/full') CHLTYPE(CLUSSDR) +
CONNAME('host1.example.com(9031)') CLUSTER('example.cluster')
   ```
Update the local queue echo to publish it within the cluster:

   ```bash
ALTER QLOCAL('echo') CLUSTER('example.cluster')
   ```
This setup ensures a distributed messaging architecture with clusters, load balancing, transmission queues, and proper routing between queue managers
