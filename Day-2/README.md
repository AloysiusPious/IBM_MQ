# IBM MQ Administration Practice — Day 2
## 🗓️ Day 2: Configuring Remote Queue Communication Between Queue Managers
## 🎯 Objective
Establish communication between two queue managers (QM1 and QM2) on the same CentOS system. Configure the necessary objects to enable message transmission from a local queue on QM1 to a local queue on QM2.​

## 🛠️ Prerequisites
IBM MQ 9.4.0.10 installed on CentOS

User with mqm group privileges

Environment variables configured using setmqenv

QM1 queue manager created (from Day 1)​

## ✅ Steps
1. Create the Second Queue Manager (QM2)
```bash
crtmqm -q QM2
strmqm QM2
```
2. Define a Local Queue on QM2
```bash
runmqsc QM2
DEFINE QLOCAL('DEST.QUEUE')
END
```
3. Configure Listener on QM2
```bash
runmqsc QM2
DEFINE LISTENER('QM2.LISTENER') TRPTYPE(TCP) PORT(1415)
START LISTENER('QM2.LISTENER')
END
```
4. Define Receiver Channel on QM2
```bash
runmqsc QM2
DEFINE CHANNEL('QM1.TO.QM2') CHLTYPE(RCVR) TRPTYPE(TCP)
END
```
5. Configure Transmission Queue on QM1
```bash
runmqsc QM1
DEFINE QLOCAL('QM2') USAGE(XMITQ)
END
```
6. Define Sender Channel on QM1
```bash
runmqsc QM1
DEFINE CHANNEL('QM1.TO.QM2') CHLTYPE(SDR) TRPTYPE(TCP) +
CONNAME('localhost(1415)') XMITQ('QM2')
END
```
7. Define Remote Queue on QM1
```bash
runmqsc QM1
DEFINE QREMOTE('REMOTE.QUEUE') RNAME('DEST.QUEUE') RQMNAME('QM2') XMITQ('QM2')
END
```
8. Start the Sender Channel on QM1
```bash
runmqsc QM1
START CHANNEL('QM1.TO.QM2')
END
```
9. Test Message Transmission
```bash
echo "Test Message" | amqsput REMOTE.QUEUE QM1
```
On QM2, retrieve the message:​

```bash
amqsget DEST.QUEUE QM2
```
📘 Explanation
Transmission Queue (QM2): A special local queue on QM1 that temporarily holds messages destined for QM2.

Sender Channel (QM1.TO.QM2): Defines the communication path from QM1 to QM2.

Receiver Channel (QM1.TO.QM2): Corresponds to the sender channel on QM2, enabling it to receive messages.

Remote Queue (REMOTE.QUEUE): An alias on QM1 that maps to the actual queue (DEST.QUEUE) on QM2.​


📚 Additional Resources
(Working with Remote Queues - IBM Documentation)[https://www.ibm.com/docs/en/ibm-mq/9.2?topic=objects-working-remote-queues]
(Putting Messages on Remote Queues - IBM Documentation)[https://www.ibm.com/docs/en/ibm-mq/9.2?topic=techniques-putting-messages-remote-queues]
(Remote Queue Definition: IBM MQ v9.2 - Medium)[https://medium.com/geekculture/remote-queue-definition-ibm-mq-v9-2-c3ec4f568dab]

