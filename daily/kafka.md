## Kafka

> 아파치 소프트웨어 재단이 스칼라로 개발한 오픈 소스 메시지 브로커 프로젝트

- 실시간 데이터 피드를 관리하기 위해 통일된, 높은 처리량, 낮은 지연시간을 지닌 플랫폼을 제공하는 것이 목적

- Kafka는 발행-구독(publish-subscribe) 모델을 기반으로 동작하며 크게 `producer`, `consumer`, `broker`로 구성

  

  

### kafka 시작하기

> windows는 `bin\windows~ `
>
> macOS, linux, unix 계열은` bin\~`
>
> <a href="http://kafka.apache.org/downloads">Kafka 설치 링크</a>

- zookeeper 서버 설치 
  - `bin\windows\zookeeper-server-start.bat config\zookeeper.properties`
- Kafka 실행
  - `bin\windows\fakfa-server-start.bat config\server.properties`

- topic 생성
  - `bin\windows\kafka-topics.bat --create --topic <topic명> --bootstrap-server localhost:9092`
- topic 리스트 확인
  - `bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092`
  - `bin\windows\kafka-topics.bat --describe --topic <topic명> --bootstrap-server localhost:9092`

- consumer 실행
  - `bin\windows\kafka-console-consumer.bat --topic <topic명> --from-beginning --bootstrap-server localhost:9092`
  - `bin\windows\kafka-console-producer.bat --topic <topic명> --bootstrap-server localhost:9092`

- kafka 브로커에서 topic 삭제
  - `bin\windows\kafka-topics.bat --delete --topic <topic명> --bootstrap-server localhost:9092`
- zookeeper에서 topic 삭제
  - `bin\windows\kafka-topics.bat --delete --topic <topic명> --zookeeper localhost:2181`

- python 모듈 설치

  - `pip install kafka-python`

- python을 이용하여 data queuing

  - `kafka_consumer.py`

    ```python
    from kafka import KafkaConsumer
    from json import loads
    import time
    
    consumer = KafkaConsumer('quickstart-events',
                             bootstrap_servers=['127.0.0.1:9092'],
                             auto_offset_reset='earliest',
                             enable_auto_commit=True,
                             group_id='my-group',
                             # value_deserializer=lambda x: loads(x.decode('utf-8')),
                             consumer_timeout_ms=1000)
    
    start = time.time()
    
    for message in consumer:
        topic = message.topic
        partition = message.partition
        offset = message.offset
        key = message.key
        value = message.value
        print("Topic:{},Partition:{},offset:{},key:{},value:{}".format(
            topic, partition, offset, key, value))
    
    print("Elasped: ", (time.time() - start))
    ```

  - `kafka_producer.py`

    ```python
    from kafka import KafkaProducer
    from json import dumps
    import time
    
    # dict (key, value) -> object
    # str -> string
    
    producer = KafkaProducer(acks=0,
                             compression_type='gzip',
                             bootstrap_servers=['127.0.0.1:9092'],
                             value_serializer=lambda x: dumps(x).encode('utf-8'))
    
    start = time.time()
    for i in range(10):
        # data = {'name': 'Yang-' + str(i)}
        data = {"schema": {"type": "struct", "fields": [{"type": "int32", "field": "id"}, {"type": "string", "field": "user_id"}, {"type": "string", "field": "pwd"}, {"type": "string", "field": "NAME"}, {
            "type": "int64", "name": "org.apache.kafka.connect.data.Timestamp", "version": 1, "field": "created_at"}], "name": "users"}, "payload": {"id": 10, "user_id": "new_test10", "pwd": "new_pwd10", "NAME": "NEW TEST USER10", "created_at": 1615349727000}}
    
        producer.send('my_topic_users', value=data)
        producer.flush()
    
    print("Done, Elasped time: ", (time.time() - start))
    ```

    

- 코드 실행

  - `python kafka_consumer.py`
  - `python kafka_producer.py`