## Amazon SQS

- **Amazon Simple Queue Service**
- https://docs.aws.amazon.com/ko_kr/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html
- AWS에서 가장 먼저 생긴 리소스



#### 메시지(message)

- XML, JSON과 같은 형태를 가진 데이터
- 256KB
- 유니코드 사용이 가능



#### 큐(queue)

- 메시지를 담는 공간
- 리전 별로 생성해야 함
- 큐 이름은 모든 리전에서 유일해야 함



#### 배치 API

- 용량이 작은 메시지를 자주 처리하면 과금이 발생하므로 메시지를 모아서 동시에 처리



#### Visibility Timeout

- 메시지를 받은 뒤 특정 시간 동안 다른 곳에서 메시지를 볼 수 없도록 하는 기능
- 여러 서버가 메시지를 처리하는 경우 동일한 메시지를 동시에(중복해서) 처리하는 것을 방지
  - **Message Available** ⇒ Visible ⇒ 메시지를 꺼내 볼 수 있는 상태
  - **Message in Fight** ⇒ Not Visible ⇒ 다른 곳에서 메시지를 보고 있어 볼 수 없는 메시지



#### Delay Delivery

- 특정 시간 동안 메시지를 받지 못하게 하는 기능
- Message in Fight 상태



#### Dead Letter Queues

- 일반적으로 메시지를 받아서 처리하면 메시지를 삭제
- 삭제되지 않고 남아 있는 경우에는 처리 실패 큐(Dead Letter Queues)로 보냄
- 일반 큐와 동일한 리전에 있어야 함



#### Polling

##### Long Polling

- 메시지가 있어면 바로 가져오고, 없으면 올 때까지 기다림
- 기본은 20초이고, 1초부터 20초까지 설정이 가능



##### Short Polling

- 메시지가 있으면 바로 가져오고, 없으면 빠져 나옴
- ReceiveMessage 요청에서 WaitTimeSeconds를 0으로 설정
  - Queue 설정에서 ReceiveMessageWaitTimeSeconds를 0으로 설정



​	<img src="..\img\image-20201008144944605.png" alt="image-20201008144944605"  />



#### Amazon SQS Standard Queue with Boto3 SDK

- https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html



**create_queue.py**

```python
import boto3

# Create SQS client
sqs = boto3.client('sqs')

# Create a SQS queue
response = sqs.create_queue(
    QueueName='mynewq',
    Attributes={
        'DelaySeconds': '5',
        'MessageRetentionPeriod': '86400'
    }
)

print(response['QueueUrl'])
```



**queue_status.py**

```python
import boto3
import time
from sqs_url import QUEUE_URL

sqs = boto3.client('sqs')

i = 0

while i < 100000:
    i = i + 1
    time.sleep(1)
    response = sqs.get_queue_attributes(
        QueueUrl=QUEUE_URL,
        AttributeNames=[
            'ApproximateNumberOfMessages',
            'ApproximateNumberOfMessagesNotVisible',
            'ApproximateNumberOfMessagesDelayed',
        ]
    )
    for attribute in response['Attributes']:
        print(
            response['Attributes'][attribute] +
            ' ' +
            attribute
        )
    print('')
    print('')
    print('')
    print('')
```

- ApproximateNumberOfMessages
  - Returns the approximate number of messages available for retrieval from the queue.
- ApproximateNumberOfMessagesDelayed
  - Returns the approximate number of messages in the queue that are delayed and not available for reading immediately. This can happen when the queue is configured as a delay queue or when a message has been sent with a delay parameter.
- ApproximateNumberOfMessagesNotVisible
  - Returns the approximate number of messages that are in flight. Messages are considered to be *in flight* if they have been sent to a client but have not yet been deleted or have not yet reached the end of their visibility window.



**slow_producer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

with open('slow_data.json', 'r') as f:
    data = json.loads(f.read())

for i in data:
    msg_body = json.dumps(i)
    response = sqs.send_message(
        QueueUrl=QUEUE_URL,
        MessageBody=msg_body,
        DelaySeconds=10,
        MessageAttributes={
            'JobType': {
                'DataType': 'String',
                'StringValue': 'NewDonor'
            },
            'Producer': {
                'DataType': 'String',
                'StringValue': 'Slow'
            }
        }
    )
    print('Added a message with 10 second delay - SLOW')
    print(response)
    time.sleep(1)
```



**fast_producer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

with open('fast_data.json', 'r') as f:
    data = json.loads(f.read())

for i in data:
    msg_body = json.dumps(i)
    response = sqs.send_message(
        QueueUrl=QUEUE_URL,
        MessageBody=msg_body,
        DelaySeconds=2,
        MessageAttributes={
            'JobType': {
                'DataType': 'String',
                'StringValue': 'NewDonor'
            },
            'Producer': {
                'DataType': 'String',
                'StringValue': 'Fast'
            }
        }
    )
    print('Added a message with 1 second delay - FAST')
    print(response)
    time.sleep(1)
```



**slow_consumer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

i = 0

while i < 10000:
    i = i + 1
    rec_res = sqs.receive_message(
        QueueUrl=QUEUE_URL,
        MessageAttributeNames=[
            'All',
        ],
        MaxNumberOfMessages=1,
        VisibilityTimeout=20,
        WaitTimeSeconds=10
    )
    del_res = sqs.delete_message(
        QueueUrl=QUEUE_URL,
        ReceiptHandle=rec_res['Messages'][0]['ReceiptHandle']
    )
    print("RECIEVED MESSAGE (SLOW CONSUMER):")
    print('FROM PRODUCER: ' + rec_res['Messages'][0]['MessageAttributes']['Producer']['StringValue'])
    print('JOB TYPE: ' + rec_res['Messages'][0]['MessageAttributes']['JobType']['StringValue'])
    print('BODY: ' + rec_res['Messages'][0]['Body'])
    print("DELETED MESSAGE (SLOW CONSUMER)")
    print("")
    time.sleep(8)
```



**fast_consumer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

i = 0

while i < 10000:
    i = i + 1
    rec_res = sqs.receive_message(
        QueueUrl=QUEUE_URL,
        MessageAttributeNames=[
            'All',
        ],
        MaxNumberOfMessages=1,
        VisibilityTimeout=5,
        WaitTimeSeconds=10
    )
    del_res = sqs.delete_message(
        QueueUrl=QUEUE_URL,
        ReceiptHandle=rec_res['Messages'][0]['ReceiptHandle']
    )
    print("RECIEVED MESSAGE (FAST CONSUMER):")
    print('FROM PRODUCER: ' + rec_res['Messages'][0]['MessageAttributes']['Producer']['StringValue'])
    print('JOB TYPE: ' + rec_res['Messages'][0]['MessageAttributes']['JobType']['StringValue'])
    print('BODY: ' + rec_res['Messages'][0]['Body'])
    print("DELETED MESSAGE (FAST CONSUMER)")
    print("")
    time.sleep(2)
```



### Amazon SQS FIFOQueue with Boto3 SDK



**표준 대기열(Standard Queue) vs FIFO 대기열(FIFO Queue) 차이**

- https://aws.amazon.com/ko/sqs/features/



**create_queue.py**

```python
import boto3

# Create SQS client
sqs = boto3.client('sqs')

# Create a SQS queue
response = sqs.create_queue(
    QueueName='mynewq.fifo',
    Attributes={
        'DelaySeconds': '5',
        'MessageRetentionPeriod': '86400',
        'FifoQueue': 'true',
        'ContentBasedDeduplication': 'true'
    }
)

print(response['QueueUrl'])
```



**fifo_producer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

with open('data.json', 'r') as f:
    data = json.loads(f.read())

for i in data:
    msg_body = json.dumps(i)
    response = sqs.send_message(
        QueueUrl=QUEUE_URL,
        MessageBody=msg_body,
        MessageAttributes={
            'JobType': {
                'DataType': 'String',
                'StringValue': 'NewDonor'
            },
            'Producer': {
                'DataType': 'String',
                'StringValue': 'Default'
            }
        },
        MessageGroupId='messageGroup1'
    )
    print("Added Message:")
    print(response)
    time.sleep(1)
```



**fifo_consumer.py**

```python
import boto3
import json
import time
from sqs_url import QUEUE_URL

# Create SQS client
sqs = boto3.client('sqs')

i = 0

while i < 10000:
    i = i + 1
    rec_res = sqs.receive_message(
        QueueUrl=QUEUE_URL,
        MessageAttributeNames=[
            'All',
        ],
        MaxNumberOfMessages=1,
        VisibilityTimeout=5,
        WaitTimeSeconds=10
    )
    time.sleep(2)
    # If our task takes too long we can't delete it
    # time.sleep(5)
    del_res = sqs.delete_message(
        QueueUrl=QUEUE_URL,
        ReceiptHandle=rec_res['Messages'][0]['ReceiptHandle']
    )
    print("RECIEVED MESSAGE:")
    print('FROM PRODUCER: ' + rec_res['Messages'][0]['MessageAttributes']['Producer']['StringValue'])
    print('JOB TYPE: ' + rec_res['Messages'][0]['MessageAttributes']['JobType']['StringValue'])
    print('BODY: ' + rec_res['Messages'][0]['Body'])
    print("DELETED MESSAGE")
    print("")
    time.sleep(1)
```

