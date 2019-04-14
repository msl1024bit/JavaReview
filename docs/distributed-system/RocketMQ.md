# RocketMQ
## �ܹ�ͼ
![�˴�����ͼƬ������](images/rocketmq-cluster-structure.png)

![�˴�����ͼƬ������](images/rocketmq-cluster-structure-2.jpg)

 - **NameServer**
NameServer��һ��������״̬�Ľڵ㣬�ɼ�Ⱥ���𣬽ڵ�֮�����κ���Ϣͬ�����������ΪEureka��
 - **Broker��master,slave��**
Broker��Ը��ӣ�Broker��ΪMaster��Slave��һ��Master���Զ�Ӧ���Slaver������һ��Slaverֻ�ܶ�Ӧһ��Master��Master��Slaver�Ķ�Ӧ��ϵͨ��ָ����ͬ��BrokerName����ͬ��BrokerId�����壬BrokerIdΪ0��ʾMaster����0��ʾSlaver��Master���Բ�������ÿ��Broker��NameServer��Ⱥ�е�**���нڵ�**���������ӣ���ʱע��Topic��Ϣ�����е�NameServer,Master��Slaver֮����ڶ�Ӧ��ͬ������
 - **Producer**
Producer��NameServer��Ⱥ�е�**����һ���ڵ�**�����ѡ�񣩽��������ӣ����ڴ�NameServerȡTopic·����Ϣ�������ṩTopic�����Master���������ӣ��Ҷ�ʱ��Master����������Producer��ȫ��״̬��
 - **Consumer��pull,push��**
Consumer��NameServer��Ⱥ�е�**����һ���ڵ�**�����ѡ�񣩽��������ӣ����ڴ�NameServerȡTopic·����Ϣ�������ṩTopic�����Master��Slaver���������ӣ��Ҷ�ʱ��Master��Slaver����������Consumer���ɴ�Master������Ϣ��Ҳ���Դ�Slave������Ϣ�����Ĺ�����Broker���þ���

## ��Ϣ����ͼ
![�˴�����ͼƬ������](images/rocketmq-message-send.png)
 
## Producer
### ��ͨ��Ϣ
**���ַ��ͷ�ʽ**

 - SYNC ͬ���ȴ�
 - ASYNC �첽�ص�
 - ONEWAY ����ȥ���ܷ���
![�˴�����ͼƬ������](images/rocketmq-ordinary-message-invoke-process.png)

**Q&A**

- ��ȡ��ͬBroker(Master)��Topic�µ����ж��������⣨��ͬ��
 ```
    ����broker1, broker2, borker3��̨broker������������Topic_A

    Broker1 �Ķ���Ϊqueue0 , queue1
    
    Broker2 �Ķ���Ϊqueue0, queue2, queue3,
    
    Broker3 �Ķ���Ϊqueue0
    
    ��Ȼһ������µ�broker�����ö���һ����
    
    ���ϵ�broker������ʱ��ע�ᵽnamesrv��Topic_A����Ϊ��6���ֱ�Ϊ��
    
    broker1_queue0, broker1_queue1,
    
    broker2_queue0, broker2_queue1, broker2_queue2,
    
    broker3_queue0,

 ```
- Producer���ʵ����ѯ����
 ```
    Producer��namesrv��ȡ�ĵ�Topic_A·����ϢTopicPublishInfo

   --List<MessageQueue>messageQueueList  //Topic_A�����еĶ���

   --AtomicIntegersendWhichQueue        //��������

   ����selectOneMessageQueue��������ѡ��һ�����Ͷ���

    (++sendWitchQueue)% messageQueueList.sizeΪ���м��ϵ��±�

    ÿ�λ�ȡqueue����ͨ��sendWhichQueue��һ��ʵ�ֶ�����queue����ѯ
    
    ������lastBrokerName��Ϊ�գ������ϴ�ѡ���queue����ʧ�ܣ����ѡ��Ӧ�ñܿ�ͬһ��queue
 ```

- Producer����Ϣϵͳ����

```
    ����ʧ�ܺ����Լ���retryTimesWhenSendFailed = 2

������Ϣ��ʱsendMsgTimeout = 3000

Producerͨ��selectOneMessageQueue������ȡһ��MessagQueue����

--topic            //Topic_A

--brokerName        //��������Ϣ�����broker

--queueId           //��������Ϣ����ָ��broker��ָ��topic�µĶ��б��

��ָ��broker��ָ��topic��ָ��queue������Ϣ

����ʧ��(1)���Դ�����������(2)���ʹ�����Ϣ����ʱ�仹û�е�3000(����), �������м������͡� 
```

#### ������Ϣ

![�˴�����ͼƬ������](images/rocketmq-transaction-message-invoke-process.png)

**�ֲ�ʽ�����ǻ��ڶ��׶��ύ��**

- һ�׶Σ���broker����һ��**prepared**����Ϣ��������Ϣ��offset����Ϣ��ַcommitLog����Ϣƫ������Prepared״̬��Ϣ�������ѷ�����Ϣok��ִ�б��������֧�� �������񷽷���Ҫʵ��rocketmq�Ļص��ӿڡ�LocalTransactionExecuter�������������߼����ش��������״̬LocalTransactionState
- ���׶Σ������걾��������ҵ��õ�����״̬�� ����offset���ҵ�commitLog�е�prepared��Ϣ��������Ϣ״̬commitType����rollbackType�� �ú���Ϣ��ӵ�commitLog�У� ��ʵ���׶�������**������Ϣ**

## Broker
### Mappedģ��
![�˴�����ͼƬ������][6]
**Consume Queue**
ÿ��topic�µ�ÿ��queue����һ����Ӧ��consumequeue�ļ����ļ���ַ��`${user.home}\store\consumequeue\${topicName}\${queueId}\${fileName}`������Ϣ���߼����У��൱���ֵ��Ŀ¼����ָ����Ϣ����Ϣ�������������ļ�commitLog�ϵ�λ�á����ͨ������ϵͳ��PageCache������߻���������ʣ���ú��ڴ�һ����Ч��

![�˴�����ͼƬ������](images/rocketmq-consume-queue-structure.png)

�����Ѷ�group�������Զ��У�������Ѷ�����ʧ�ܣ����͵�retry���Ѷ����У�����ͼ�е�`%RETRY%ConsumerGroupA`��
�����Ѷ�group�������Ŷ��У�������Ѷ����Գ���ָ���������������Ŷ��У� ����ͼ�е�`%DLQ%ConsumerGroupA`��

Consume Queue�д洢��Ԫ��һ��**20�ֽ�**�����Ķ��������ݣ�**˳��д˳���**������ͼ��ʾ��
![�˴�����ͼƬ������](images/rocketmq-consume-queue-storage-unit.png)

consumequeue�ļ��洢��Ԫ��ʽ

- CommitLog Offset��ָ������Ϣ��Commit Log�ļ��е�ʵ��ƫ����
- Size�洢����Ϣ�Ĵ�С
- Message Tag HashCode�洢��Ϣ��Tag�Ĺ�ϣֵ����Ҫ���ڶ���ʱ��Ϣ���ˣ�����ʱ���ָ����Tag�������HashCode�����ٲ��ҵ����ĵ���Ϣ��

**Commit Log**
�ļ���ַ��`${user.home} \store\${commitlog}\${fileName}`,һ����Ϣ�洢��Ԫ�����ǲ����ģ�**˳��д���������**,commitLogÿ���ļ��Ĵ�СĬ��1G =102410241024

**MapedFile**
MapedFile ��PageCache�ļ���װ�����������ļ����ڴ��е�ӳ���Լ����ڴ����ݳ־û��������ļ��У�������д����Ҫ��osϵͳ��ҳ��СΪ4k

**MapedFileQueue**
�����ж���ļ���MapedFile����ɣ��ɼ��϶���List��ʾ�������У�ǰ�潲���ļ���������Ϣ�ڴ��ļ����г�ʼƫ�������ź���������һ����������Ϣ����

## Master && Slaver
![�˴�����ͼƬ������][9]

- WriteSocketService ��slaveͬ��commitLog�����߳�
- ReadSocketService ��ȡslaveͨ��HAClient��master����ͬ��commitLog������ƫ����phyOffsetֵ

## Consumer
### consumer���ؾ���
���Ѷ˻�ͨ��**RebalanceService**�̣߳�10������һ�λ���topic�µ����ж��и���
���ؾ������consumer��push��pullģʽ��Ч��

������⣺
1. Ҫ�����ؾ��⣬����Ҫ���ľ����ź��ռ���
��ν�ź��ռ������ǵ�֪��ÿһ��consumerGroup����Щconsumer����Ӧ��topic��˭���ź��ռ���ΪClient���ź��ռ���Broker���ź��ռ��������֡�
2. ���ؾ������Client�˴���
���������ǣ������߿ͻ���������ʱ����rebalanceImplʵ����ͬʱ����������Ϣ���rebalanceImplʵ�������У�����Ҳ�Ǻ���Ҫ��һ������ -- ͨ��������Ϣ����ͣ���ϱ��Լ�������Broker��ע��RegisterConsumer���ȴ���������׼����֮����Client�˲���ִ�еĸ��ؾ�������̴߳�Broker�˻�ȡһ��ȫ����Ϣ����consumerGroup�����е�����Client����Ȼ�������Щȫ����Ϣ����ȡ��ǰ�ͻ��˷��䵽�����Ѷ��С�

### consumer push��pull
consumer����Ϊ2�ࣺMQPullConsumer��MQPushConsumer����ʵ���ʶ�����ģʽ��pull������consumer��ѯ��broker��ȡ��Ϣ�� �����ǣ�

- ��ģʽ�consumer����ѯ���̷�װ�ˣ���ע��MessageListener��������ȡ����Ϣ�󣬻���MessageListener��consumeMessage()�����ѣ����û����ԣ��о���Ϣ�Ǳ����ͣ�push�������ġ�
��ģʽֱ��ͨ��������ͨ�����͵��ͻ��ˡ����ŵ��Ǽ�ʱ��һ�������ݱ�����ͻ��������ܸ�֪��������Կͻ�����˵�߼��򵥣�����Ҫ��������������Щ�߼�����ȱ���ǲ�֪���ͻ��˵������������������ܵ������ݻ�ѹ�ڿͻ��ˣ�����������
- ��ģʽ�ȡ��Ϣ�Ĺ�����Ҫ�û��Լ�д������ͨ���������ѵ�Topic�õ�MessageQueue�ļ��ϣ�����MessageQueue���ϣ�Ȼ�����ÿ��MessageQueue����ȡ��Ϣ��һ��ȡ��󣬼�¼�ö�����һ��Ҫȡ�Ŀ�ʼoffset��ֱ��ȡ���ˣ��ٻ���һ��MessageQueue��
��ģʽ�ŵ��Ǵ˹����ɿͻ��˷������󣬹ʲ�������ģʽ�����ݻ�ѹ�����⡣ȱ���ǿ��ܲ�����ʱ���Կͻ�����˵��Ҫ����������ȡ����߼�����ʱȥ��������Ƶ����ô���Ƶȵȡ�

## RocketMQ�߿��ã���Ҫ��namesrv��broker�ĸ߿��ã�

![�˴�����ͼƬ������](images/rocketmq-high-available.jpeg)
### NameServer�߿���
- ��� Namesrv ֮�䣬û���κι�ϵ������������ Zookeeper �� Leader/Follower �Ƚ�ɫ����������ͨ��������ͬ����ͨ�� Broker **ѭ��ע��**��� Namesrv��
- Producer��Consumer �� Namesrv�б�**ѡ��һ��������**�Ľ���ͨ�š�

### Broker�߿���
������� Broker���� **�γ� ��Ⱥ** ʵ�ָ߿���

- ÿ�����飬Master�ڵ� ���Ϸ����µ� CommitLog �� Slave�ڵ㡣 Slave�ڵ� �����ϱ����ص� CommitLog �Ѿ�ͬ������λ�ø� Master�ڵ㡣
- Broker���� �� Broker���� ֮��û���κι�ϵ��������ͨ��������ͬ��

��Ⱥ�ڣ�Master�ڵ� ���������ͣ�`Master_SYNC`��`Master_ASYNC`��ǰ���� Producer ������Ϣʱ���ȴ� Slave�ڵ� �洢��Ϻ��ٷ��ط��ͽ���������߲���Ҫ�ȴ���


## ��Ϣ����
### �ŵ�
����첽�����壬��˵�����ֵ�Ӧ�ó���
### ȱ��
- ϵͳ�����Խ��͡���Ϊ�������MQ����
- ϵͳ���Ӷ���ߣ���Ҫ������Ϣ�ظ�����ʧ��˳�������
- ����һ��������

### Kafka��ActiveMQ��RabbitMQ��RocketMQ ��ʲô��ȱ�㣿

| ���� | ActiveMQ | RabbitMQ | RocketMQ | Kafka |
|---|---|---|---|---|
| ���������� | �򼶣��� RocketMQ��Kafka ��һ�������� | ͬ ActiveMQ | 10 �򼶣�֧�Ÿ����� | 10 �򼶣������£�һ����ϴ��������ϵͳ������ʵʱ���ݼ��㡢��־�ɼ��ȳ��� |
| topic ��������������Ӱ�� | | | topic ���Դﵽ����/��ǧ�ļ������������н�С���ȵ��½������� RocketMQ ��һ�����ƣ���ͬ�Ȼ����£�����֧�Ŵ����� topic | topic �Ӽ�ʮ�����ٸ�ʱ���������������½�����ͬ�Ȼ����£�Kafka ������֤ topic ������Ҫ���࣬���Ҫ֧�Ŵ��ģ�� topic����Ҫ���Ӹ���Ļ�����Դ |
| ʱЧ�� | ms �� | ΢�뼶������ RabbitMQ ��һ���ص㣬�ӳ���� | ms �� | �ӳ��� ms ������ |
| ������ | �ߣ��������Ӽܹ�ʵ�ָ߿��� | ͬ ActiveMQ | �ǳ��ߣ��ֲ�ʽ�ܹ� | �ǳ��ߣ��ֲ�ʽ��һ�����ݶ����������������崻������ᶪʧ���ݣ����ᵼ�²����� |
| ��Ϣ�ɿ��� | �нϵ͵ĸ��ʶ�ʧ���� | �������� | ���������Ż����ã��������� 0 ��ʧ | ͬ RocketMQ |
| ����֧�� | MQ ����Ĺ��ܼ����걸 | ���� erlang ����������������ǿ�����ܼ��ã���ʱ�ܵ� | MQ ���ܽ�Ϊ���ƣ����Ƿֲ�ʽ�ģ���չ�Ժ� | ���ܽ�Ϊ�򵥣���Ҫ֧�ּ򵥵� MQ ���ܣ��ڴ����������ʵʱ�����Լ���־�ɼ������ģʹ�� |


## �ظ���Ϣ�Լ���֤��Ϣ���ѵ��ݵ���
- ��������д Redis����û�����ˣ�����ÿ�ζ��� set����Ȼ�ݵ��ԡ�
- �������߷���ÿ�����ݵ�ʱ�������һ��ȫ��Ψһ�� id�����ƶ��� id ֮��Ķ�������redisά��һ��set���ϴ洢���ȫ��id��Ȼ�����������ѵ���֮���ȸ������ id ȥRedis ���һ�£�֮ǰ���ѹ������û�����ѹ�����ʹ���Ȼ����� id д Redis��
- ���ݿ��Ψһ������֤�ظ����ݲ����ظ��������

## ��δ�����Ϣ��ʧ�����⣨��α�֤��Ϣ�Ŀɿ��Դ��䣿��
Ӱ����Ϣ�ɿ��Եļ��������

- broker�����ر�
- broker�쳣crash
- OS crash
- �������磬���������ָ��������
- �������ܿ�����Ӳ���𻵣�
- �����豸��

ǰ��1-4�������������**Ӳ����Դ�������ָ�**�������rocketmq��������������ܱ�֤��Ϣ��������ʧ�������ݣ�����ˢ�̷�ʽ��ͬ�������첽����

5-6���������**�ڵ�����ϣ��Ҳ��ָܻ�**��һ���������ڴ˵����ϵ���Ϣȫ����ʧ��rocketmq������������£�ͨ��**�첽����**���ɱ�֤99%����Ϣ������ͨ��**ͬ��˫д**����������ȫ���ⵥ�㣬����Ӱ�����ܣ��ʺ϶���Ϣ�ɿ���Ҫ�󼫸ߵĳ���������Ǯ��ص�Ӧ�á�

## ��α�֤��Ϣ˳����Ϣ��
![�˴�����ͼƬ������](images/rocketmq-ordered-message.png)

### Producer��
Producer��ȷ����Ϣ˳��ΨһҪ����������ǽ���ͬ����Ϣ·�ɵ���ͬ��messageQueue����RocketMQ�У�ͨ��`MessageQueueSelector`��ʵ�ַ�����ѡ��

### Consumer��
![�˴�����ͼƬ������](images/rocketmq-ordered-message-consumer-side.png)

1. PullMessageService���̵߳Ĵ�Broker��ȡ��Ϣ
2. PullMessageService����Ϣ��ӵ�ProcessQueue�У�ProcessMessage��һ����Ϣ�Ļ��棩��֮���ύһ����������ConsumeMessageOrderService
3. ConsumeMessageOrderService���߳�ִ�У�ÿ���߳���������Ϣʱ��Ҫ�õ�MessageQueue����
4. �õ���֮���ProcessQueue�л�ȡ��Ϣ

## ������һ����Ϣ���У�
1. ������� mq ��֧�ֿ������԰ɣ�������Ҫ��ʱ��������ݣ��Ϳ�������������������������ô�㣿��Ƹ��ֲ�ʽ��ϵͳ�£�����һ�� kafka ��������broker -> topic -> partition��ÿ�� partition ��һ���������ʹ�һ�������ݡ����������Դ�����ˣ��򵥰����� topic ���� partition��Ȼ��������Ǩ�ƣ����ӻ��������Ϳ��Դ�Ÿ������ݣ��ṩ���ߵ��������ˣ�
2. �����ÿ���һ����� mq ������Ҫ��Ҫ��ش��̰ɣ��ǿ϶�Ҫ�ˣ�����̲��ܱ�֤����̹������ݾͶ��ˡ�������̵�ʱ����ô�䰡��˳��д��������û�д��������д��Ѱַ����������˳���д�������Ǻܸߵģ������ kafka ��˼·��
3. ����㿼��һ����� mq �Ŀ����԰�������¶�������ο�֮ǰ�������Ǹ����ڽ���� kafka �ĸ߿��ñ��ϻ��ơ��ั�� -> leader & follower -> broker ��������ѡ�� leader ���ɶ������
4. �ܲ���֧������ 0 ��ʧ�������Եģ��ο�����֮ǰ˵���Ǹ� kafka �����㶪ʧ������