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
