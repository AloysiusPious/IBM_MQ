📅 Day 1: Setting Up Your First Queue Manager
🎯 Objective
Establish a foundational IBM MQ environment by creating a queue manager, defining a local queue, and verifying message operations.​

🛠️ Prerequisites
IBM MQ 9.4.0.10 installed on CentOS

User with mqm group privileges

Environment variables configured using setmqenv​
IBM - United States

✅ Steps
1. Create a Queue Manager
bash
Copy
Edit
crtmqm -q QM1
-q enables the queue manager to start automatically upon system boot.​
IBM - United States
+7
IBM - United States
+7
IBM - United States
+7

2. Start the Queue Manager
bash
Copy
Edit
strmqm QM1
3. Define a Local Queue
bash
Copy
Edit
runmqsc QM1
DEFINE QLOCAL('TEST.QUEUE')
END
4. Display the Queue
bash
Copy
Edit
runmqsc QM1
DISPLAY QLOCAL('TEST.QUEUE')
END
5. Send a Test Message
bash
Copy
Edit
echo "Hello MQ" | amqsput TEST.QUEUE QM1
After executing, type your message and press Enter. To end input, press Ctrl+D.​

6. Retrieve the Message
bash
Copy
Edit
amqsget TEST.QUEUE QM1
This command reads the message from the queue.​

7. Stop the Queue Manager
bash
Copy
Edit
endmqm QM1
📘 Explanation
Queue Manager (QM1): Central component managing queues and channels.

Local Queue (TEST.QUEUE): Stores messages for applications to retrieve.

amqsput and amqsget: Sample programs to put and get messages, useful for testing.​

📚 Additional Resources
IBM MQ 9.4 Quick Start Guide

IBM MQ 9.4 Scenarios PDF

