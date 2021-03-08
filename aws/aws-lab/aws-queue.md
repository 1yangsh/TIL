## SQS

> Simple Queue Service

- Queue
  - 메시지를 담는 공간
  - 리전 별로 생성해야 함
  - HTTP 프로토콜을 이용해서 다른 리전끼리 메시지를 주고 받을 수 있음
  - 큐 이름은 모든 리전에서 유일해야 함
  - 큐에 담을 수 있는 메시지의 개수는 무제한
  - 연속해서 30일 동안 아무 요청이 발생하지 않으면 AWS가 큐를 삭제
  - 같은 리전 안에서의 데이터 전송은 무료
  - 다른 리전에 있는 인스턴스와 메시지를 주고 받으면 데이터 요금이 과금

- Message
  - XML 또는 JSON 형태
  - 최대 256KB
  - 유니코드 문자 사용 가능
  - 초 단위로 보관 기간 설정이 가능
  - 고유한 ID가 부여됨
  - 3~4KB의 메시지도 256KB로 책정 (과금)
    - 용량이 작은 메시지를 처리하는 것 보다는 메시지를 모아서 Batch API로 처리하는 것이 요금 효율적
- Batch API
  - 한번에 최대 10개의 메시지(최대 256KB)를 동시에 처리  



#### SQS Delay Queue

**![img](aws-queue.assets/iYR0l8Ban1_e6O4lSqJ-LS2wus8avmLOvX2ECcxahHtPXB4UFXY5v8qRLgckjnem2TAQz5kzma-J9eyi3TfCPSWQNlxx3TUeCcXhwT9J4KjlXiGX1G3d3HOEIrT_-Ja5w-ghRXU9)**

- Visibility timeout
  - 메시지를 받은 뒤 특정 시간 동안 다른 곳에서 동일한 메시지를 다시 꺼내 볼 수 있게 또는 없게 하는 기능
  - 큐 하나에 여러 서버에 메시지를 받을 때 동일한 메시지를 동시에 처리하는 것을 방지
  - Message Avaliable (Visible) 
    - ⇒ 메시지를 꺼내서 볼 수 있는 상태의 메시지 개수
  - Message in Fight (Not Visible) 
    - ⇒ 다른 곳에서 메시지를 보고 있어 현재 볼 수 없는 메시지의 개수

![img](aws-queue.assets/YfBWTrp4npbQjAKh_MqJDJ1rak9F_6BpcOJ69s6c7hs08qvWMrMxFcVQA2HNkhxuGtDg_Qc6gK7wNQFXcYZmWHOgFGZRCNh4h32fk6w_sxmWCERETFGyZcdnY1jAtuJpM5HC9BYS)



- Delay Delivery
  - 특정 시간 동안 메시지를 맏지 못하게 하는 기능
  - 지연되는 시간 동안에는 Message in Fight에 포함
- Dead Letter Queue
  - 일반적으로 메시지를 받고 작업이 완료되면 메시지를 삭제
  - 설정한 횟수를 초과하여 메시지를 받았는데 삭제되지 않고 남아 있으면 처리 실패 큐로 보냄
  - 일반 큐 하나에 여러 처리 실패 큐를 연결할 수 있으며, 일반 큐와 처리 실패 큐는 같은 리전에 있어야 함
- Short Polling
  - 메시지 받기 요청을 하면 결과를 바로 받음
  - 메시지가 있으면 메시지를 가져오고 없으면 빠져 나옴
  - ReceiveMessage 요청에서` WaitTimeSeconds를 0`으로 했을 때
  - Queue 설정의 `ReceiveMessageWaitTimeSeconds를 0`으로 했을 때
- Long Poling
  - 메시지가 있으면 바로 가져오고 없으면 올 때까지 기다림
  - 1초부터 20초까지 설정이 가능 (기본은 20초)
  - ReceiveMessage 요청에서 WaitTimeSeconds를 0보다 크면 큐 설정의 ReceiveMessageWaitTimeSeconds 값 보다 우선으로 처리

<br/>

---

<br/>

## LAB3 : Working with AWS SQS Standard Queues

#### #1 첫번째 터미널에서 표준 SQS 큐를 생성

- PS C:\Users\i> `ssh cloud_user@35.171.155.18`

  ```
  The authenticity of host '35.171.155.18 (35.171.155.18)' can't be established.
  ECDSA key fingerprint is SHA256:FyosBNARt6R3YHhIrYGEPysYqNC+08LqeHJND4dM0ls.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added '35.171.155.18' (ECDSA) to the list of known hosts.
  Password: B^C@6oOk	⇐ LAB에서 제공한 패스워드
  
         __|  __|_  )
         _|  (     /   Amazon Linux 2 AMI
        ___|\___|___|
  
  https://aws.amazon.com/amazon-linux-2/
  ```

  

- [cloud_user@ip-10-0-1-23 ~]$ `ls`

  ```
  create_queue.py   fast_data.json    purge_queue.py   slow_consumer.py  slow_producer.py
  fast_consumer.py  fast_producer.py  queue_status.py  slow_data.json    sqs_url.py
  ```

- [cloud_user@ip-10-0-1-23 ~]$ `cat create_queue.py`

  ```python
  import boto3
  
  # Create SQS client
  sqs = boto3.client('sqs')
  
  # Create a SQS queue
  response = sqs.create_queue(
      QueueName='mynewq',
      Attributes={
          'DelaySeconds': '5',
          'MessageRetentionPeriod': '86400'	# 24시간
      }
  )
  
  print(response['QueueUrl'])
  ```

- [cloud_user@ip-10-0-1-23 ~]$ `python3 create_queue.py`

  ```
  https://queue.amazonaws.com/850718585343/mynewq
  ```

![img](aws-queue.assets/9UgbJZsGivn3JbdYxBl7MIoCm3ebzXTgLXe0Lty6CuS9Zz2eUUz1lIiWIXiwVhOMJTW_2TmKRDLjL08WH9AGgoLQDG5wjR4yitOZ-UCIJvaztr6K86wTmibKolkJ504uDVkkh_7R)

#### # create_queue() 참고 링크

- https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#SQS.Client.create_queue

  ```
  response = client.create_queue(
      QueueName='string',
      Attributes={
          'string': 'string'
      },
      tags={
          'string': 'string'
      }
  )
  ```

- DelaySeconds – The length of time, in seconds, for which the delivery of all messages in the queue is delayed. Valid values: An integer from 0 to 900 seconds (15 minutes). Default: 0.

- MessageRetentionPeriod – The length of time, in seconds, for which Amazon SQS retains a message. Valid values: An integer from 60 seconds (1 minute) to 1,209,600 seconds (14 days). Default: 345,600 (4 days).

#### #2 생성된 큐 URL을 sqs_url.py 파일에 등록 ⇒ 다른 어플리케이션에서 참조하기 위해서 사용

- [cloud_user@ip-10-0-1-23 ~]$ `sudo vi sqs_url.py`

  ```
  QUEUE_URL = 'https://queue.amazonaws.com/850718585343/mynewq'
  ```

  

#### #3 새 터미널이서 큐 모니터링

- [cloud_user@ip-10-0-1-23 ~]$ `python3 queue_status.py`

  ```
  0 ApproximateNumberOfMessages
  0 ApproximateNumberOfMessagesNotVisible
  0 ApproximateNumberOfMessagesDelayed
  
  
  
  0 ApproximateNumberOfMessages
  0 ApproximateNumberOfMessagesNotVisible
  0 ApproximateNumberOfMessagesDelayed
  	:
  ```

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

  

#### # get_queue_attributes

- https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#SQS.Client.get_queue_attributes

  ```
  response = client.get_queue_attributes(
      QueueUrl='string',
      AttributeNames=[
          'All'|'Policy'|'VisibilityTimeout'|'MaximumMessageSize'|'MessageRetentionPeriod'|'ApproximateNumberOfMessages'|'ApproximateNumberOfMessagesNotVisible'|'CreatedTimestamp'|'LastModifiedTimestamp'|'QueueArn'|'ApproximateNumberOfMessagesDelayed'|'DelaySeconds'|'ReceiveMessageWaitTimeSeconds'|'RedrivePolicy'|'FifoQueue'|'ContentBasedDeduplication'|'KmsMasterKeyId'|'KmsDataKeyReusePeriodSeconds'|'DeduplicationScope'|'FifoThroughputLimit',
      ]
  )
  
  ```

  - ApproximateNumberOfMessages – Returns the approximate number of messages available for retrieval from the queue.
  - ApproximateNumberOfMessagesDelayed – Returns the approximate number of messages in the queue that are delayed and not available for reading immediately. This can happen when the queue is configured as a delay queue or when a message has been sent with a delay parameter.
  - ApproximateNumberOfMessagesNotVisible – Returns the approximate number of messages that are in flight. Messages are considered to be in flight if they have been sent to a client but have not yet been deleted or have not yet reached the end of their visibility window.   



#### #3 세번째 터미널에서 큐로 메시지를 전송

- `slow_producer.py`

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
          DelaySeconds=10, 			# delaySecond 10초
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

- `python3 slow_producer.py`   



#### # send_message

- https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#SQS.Client.send_message

  ```
  response = client.send_message(
      QueueUrl='string',
      MessageBody='string',
      DelaySeconds=123,
      MessageAttributes={
          'string': {
              'StringValue': 'string',
              'BinaryValue': b'bytes',
              'StringListValues': [
                  'string',
              ],
              'BinaryListValues': [
                  b'bytes',
              ],
              'DataType': 'string'
          }
      },
      MessageSystemAttributes={
          'string': {
              'StringValue': 'string',
              'BinaryValue': b'bytes',
              'StringListValues': [
                  'string',
              ],
              'BinaryListValues': [
                  b'bytes',
              ],
              'DataType': 'string'
          }
      },
      MessageDeduplicationId='string',
      MessageGroupId='string'
  )
  ```

  

- `fast_producer.py`

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
          DelaySeconds=2,           # delaySecond 2초
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



#### #4 네번째 터미널에서 큐로부터 메시지를  빠르게 받아 오는 것을 실행

- `fast_consumer.py`

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
          VisibilityTimeout=5,		# 5초
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

  - https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#SQS.Client.receive_message

    ```
    response = client.receive_message(
        QueueUrl='string',
        AttributeNames=[
            'All'|'Policy'|'VisibilityTimeout'|'MaximumMessageSize'|'MessageRetentionPeriod'|'ApproximateNumberOfMessages'|'ApproximateNumberOfMessagesNotVisible'|'CreatedTimestamp'|'LastModifiedTimestamp'|'QueueArn'|'ApproximateNumberOfMessagesDelayed'|'DelaySeconds'|'ReceiveMessageWaitTimeSeconds'|'RedrivePolicy'|'FifoQueue'|'ContentBasedDeduplication'|'KmsMasterKeyId'|'KmsDataKeyReusePeriodSeconds'|'DeduplicationScope'|'FifoThroughputLimit',
        ],
        MessageAttributeNames=[
            'string',
        ],
        MaxNumberOfMessages=123,
        VisibilityTimeout=123,
        WaitTimeSeconds=123,
        ReceiveRequestAttemptId='string'
    )
    
    ```

    



#### #5 다섯번째 터미널에서 큐로부터 메시지를 천천히 받아 오는 것을 실행

- `slow_consumer.py`

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
          VisibilityTimeout=20,    # 20초
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

<br/>

---

<br/>

## LAB4 : Working with AWS SQS FIFO Queues

- 대기열 유형
  - https://aws.amazon.com/ko/sqs/features/

![img](aws-queue.assets/bIipD8RzP3tpskNkLZUwCxKFxgdmS2Wi2k9FLXmrRgN3PdIw7OnI6zLcjF9UT0iyakt5ZoMsexHAHkao_f9sDZU4Qnp1bpmm0V2swE9v8QRrYHf5pCvi-M3I0roqCISmdNCQOqu7)

  

