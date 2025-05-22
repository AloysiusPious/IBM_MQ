#📘 IBM MQ Administration Practice — Day 3
##🗓️ Day 3: Implementing Channel Authentication and User Authorization
#🎯 Objective
Enhance the security of your IBM MQ environment by configuring channel authentication (CHLAUTH) rules and setting appropriate user authorizations.
mqtechconference.com

#🛠️ Prerequisites
IBM MQ 9.4.0.10 installed on CentOS

User with mqm group privileges

Environment variables configured using setmqenv

Queue manager QM1 created and running

#✅ Steps
1. Create a Non-Privileged User
Create a new user appuser to be used for channel connections:

bash```
sudo useradd appuser
sudo passwd appuser
```
Ensure appuser is not part of the mqm group to maintain least privilege.

2. Create a Server-Connection Channel
Define a new server-connection channel APP.SVRCONN:
IBM
+6
Eclipse
+6
IBM
+6

bash
Copy
Edit
runmqsc QM1
DEFINE CHANNEL('APP.SVRCONN') CHLTYPE(SVRCONN) TRPTYPE(TCP)
END
3. Configure Channel Authentication (CHLAUTH) Rules
Map connections from a specific IP address to the appuser user ID:
IBM

bash
Copy
Edit
runmqsc QM1
SET CHLAUTH('APP.SVRCONN') TYPE(ADDRESSMAP) ADDRESS('192.168.1.100') MCAUSER('appuser')
END
This rule ensures that any connection to APP.SVRCONN from IP 192.168.1.100 will run under the appuser identity.
Eclipse

4. Set User Authorizations
Grant appuser the necessary permissions:

bash
Copy
Edit
runmqsc QM1
SET AUTHREC OBJTYPE(QMGR) PRINCIPAL('appuser') AUTHADD(CONNECT, INQ)
SET AUTHREC PROFILE('APP.QUEUE') OBJTYPE(QUEUE) PRINCIPAL('appuser') AUTHADD(PUT, GET)
END
Ensure that APP.QUEUE exists or create it:

bash
Copy
Edit
runmqsc QM1
DEFINE QLOCAL('APP.QUEUE')
END
5. Test the Configuration
Attempt to connect to QM1 using the APP.SVRCONN channel from IP 192.168.1.100 and perform put/get operations on APP.QUEUE.
Eclipse
+1
IBM
+1

Use the amqsputc and amqsgetc utilities for testing:

bash
Copy
Edit
amqsputc APP.QUEUE QM1
amqsgetc APP.QUEUE QM1
Ensure that the operations succeed, confirming that the channel authentication and user authorizations are correctly configured.
IBM

📘 Explanation
CHLAUTH Rules: Channel authentication records (CHLAUTH) provide a mechanism to control access to channels based on various criteria such as IP address, user ID, and SSL/TLS credentials. 
IBM
+6
IBM
+6
mqtechconference.com
+6

MCAUSER: The MCAUSER attribute specifies the user ID under which the channel runs. Mapping connections to a specific MCAUSER ensures that the channel operates with the intended permissions.
Stack Overflow
+1
IBM
+1

Authorization Records: AUTHREC commands are used to grant or revoke permissions for users on various MQ objects like queues and the queue manager itself.
Eclipse
+1
IBM
+1

📚 Additional Resources
IBM MQ Channel Authorization

Securing IBM MQ

MQ Channel Authentication Records (PDF)

