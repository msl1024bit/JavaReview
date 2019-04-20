# MySQL

## Mybatis
### Mybatis����
 - ��Ĭ�Ͽ�����һ�����棺**��������ͬһ��SqlSession**����ͬһ��sqlSession������ִ����ͬ��sql��䣬��һ��ִ����ϻὫ���ݿ��в�ѯ������д�����棨�ڴ棩���ڶ��λ�ӻ����л�ȡ���ݽ����ٴ����ݿ��ѯ���Ӷ���߲�ѯЧ�ʡ���һ��sqlSession�������sqlSession�е�һ������Ҳ�Ͳ������ˡ�
 - �������棺**mapper����Ļ���**�����SqlSessionȥ����ͬһ��Mapper��sql��䣬���SqlSessionȥ�������ݿ�õ����ݻ���ڶ����������򣬶��SqlSession���Թ��ö������棬���������ǿ�SqlSession�ġ���ͬ��sqlSession����ִ����ͬnamespace�µ�sql�������sql�д��ݲ���Ҳ��ͬ������ִ����ͬ��sql��䣬��һ��ִ����ϻὫ���ݿ��в�ѯ������д�����棨�ڴ棩���ڶ��λ�ӻ����л�ȡ���ݽ����ٴ����ݿ��ѯ���Ӷ���߲�ѯЧ�ʡ�MybatisĬ��û�п�������������Ҫ��settingȫ�ֲ��������ÿ�����������

### Mybatis��ִ������
**ע�⣺**���л�ȡmapper��ͨ��JDK�Ķ�̬��������mapperProxy��������Ȼ��ִ�ж�Ӧ��sql�������ײ���ͨ��executorʵ�־����sql������

**ʱ��ͼ��**
1. SqlSessionFactory �� SqlSession.
![�˴�����ͼƬ������][1]
2. ����֮MapperProxy:
![�˴�����ͼƬ������](images/mysql-mybatis-mapper-proxy.png)
3. Excutor:
![�˴�����ͼƬ������](images/mysql-mybatis-executor.png)


## MySQL����
### ������ѡ��
- �������������������������
- ����������Where�Ӿ��е��ֶΣ��ر��Ǵ�����ֶΣ�Ӧ�ý���������
- ����Ӧ�ý���ѡ���Ըߵ��ֶ��ϣ�
- ����Ӧ�ý���С�ֶ��ϣ����ڴ���ı��ֶ����������ֶΣ���Ҫ��������
- Ƶ���������ݲ����ı�����Ҫ����̫���������

### ����ʧЧ
- ʹ�õ������в��Ǹ��������б��еĵ�һ����
- Ӧ���������� where �Ӿ��ж��ֶν��� null ֵ�ж�
- Ӧ���������� where �Ӿ���ʹ��!=��<>������
- Ӧ���������� where �Ӿ���ʹ�� or ���������� (��or�ָ�����������orǰ�������е����������������������û����������ô�漰�����������ᱻ�õ�)��������union all�滻
- �������������������ַ������У�ʹ�÷Ǵ�ͷ��ĸ%����
- Ӧ���������� where �Ӿ��ж��ֶν��б���ʽ�������ߺ�������

### ������Ψһ��������
- ����һ���ᴴ��һ��Ψһ������������Ψһ�������в�һ����������
- ����������Ϊ��ֵ��Ψһ������������ֵ��
- һ����ֻ����һ�����������ǿ����ж��Ψһ������
- �������Ա�����������Ϊ�����Ψһ�����в����ԣ�
- ������һ��Լ������Ψһ������һ���������Ǳ����������ݽṹ�������б��ʵĲ��

### MySQL explain����
![�˴�����ͼƬ������](images/mysql-explain.png)

**id����ӳ���Ǳ��Ķ�ȡ��˳�򣬻��ѯ��ִ��select�Ӿ��˳��**
С����Զ������������������
1. id��ͬ��ִ��˳�����������µ�
2. id��ͬ��������Ӳ�ѯ��id��Ż������idֵԽ�����ȼ�Խ�ߣ�Խ�ȱ�ִ��
3. id������ͬ�ģ�Ҳ���ڲ�ͬ�ģ��������У�idԽ��Խ��ִ�У����id��ͬ�ģ���������˳��ִ��
![�˴�����ͼƬ������](images/mysql-explain-id.png)

**select_type����ӳ����Mysql����Ĳ�ѯ����**

 - simple���򵥵�select��ѯ����ѯ�в������Ӳ�ѯ��union��
 - primary����ѯ���������κθ��ӵ��ֲ��֣�������ѯ���Ϊprimary��
 - subquery��select��where�б��е��Ӳ�ѯ��
 - derived������������from�б��а������Ӳ�ѯ��Mysql��ݹ�ִ����Щ�Ӳ�ѯ���ѽ��������ʱ���
 - union�����ڶ���select������union���򱻱��Ϊunion����union������from�־���Ӳ�ѯ�У����select�������Ϊderived
 - union result��union��Ľ����

**table����ӳ��һ�������ǹ������ű���**
**type��������������**
��ӳsql�Ż���״̬�����ٴﵽrange��������ܴﵽref
��ѯЧ�ʣ�system > const > eq_ref > ref > range > index > all
������������system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index >all����

 - system���ӵ���ֻ���һ�м�¼������ϵͳ����������const���͵�������һ�㲻�����
 - const����ѯ�����õ��˳�����ͨ������һ�ξ��ҵ�������ʹ��primary key��unique�����г��֡�
![�˴�����ͼƬ������](images/mysql-explain-type-const.png)
 - eq_ref��Ψһ������ɨ�裬����ÿ��������������ֻ��һ����¼��֮ƥ�䣬������������Ψһ����ɨ�衣
 - ref����Ψһ������ɨ�裬����ƥ��ĳ������ֵ�������У�������Ҳ��һ���������ʣ������ܻ��ҵ���������������У���eq_ref�Ĳ����eq_refֻƥ����һ����¼��
 - range��ֻ����������Χ���У�ʹ��һ��������ѡ���С�key����ʾʹ�����ĸ�������һ������where����г�����between��<��>��in�ȵĲ�ѯ�����ַ�Χɨ������ɨ���ȫ��ɨ��Ҫ�ã���Ϊ��ֻ��Ҫ��ʼ��������ĳһ�㣬����������һ�㣬����ɨ��ȫ����������eq_ref��ref����������ɸѡ�������ǹ̶�ֵ���Ƿ�Χ��
![�˴�����ͼƬ������](images/mysql-explain-type-range.png)
 - index��full Index scan��index��all������Ϊindex����ֻ��������������ͨ����all�죬��Ϊ�����ļ�ͨ���������ļ�С��
![�˴�����ͼƬ������](images/mysql-explain-type-index.png)
 - all��ȫ��ɨ�裬�����ѯ�������ܴ�ʱ��ȫ��ɨ��Ч���Ǻܵ͵ġ�

**possible_keys��key��key_len����ӳʵ���õ����ĸ������������Ƿ�ʧЧ**

 - possible_keys��Mysql�Ʋ�����õ�����������Щ������һ������ѯʵ��ʹ��
 - key��ʵ��ʹ�õ���������Ϊnull�������û������������ʧЧ������ѯ����ʹ���˸������������������������key�б��С�����������select������ֶκ����������ĸ�����˳��һ�£�
![�˴�����ͼƬ������](images/mysql-explain-key.png)
 - key_len����ʾ������ʹ�õ��ֽ�������ͨ�����м����ѯ��ʹ�õ������ĳ��ȡ�ͬ���Ĳ�ѯ����£�����Խ��Խ�á�key_len��ʾ��ֵΪ�����ֶε������ܳ��ȣ�����ʵ��ʹ�ó��ȣ���key_len�Ǹ��ݱ����������ã�����ͨ�����ڼ������ġ�
 
**ref����ӳ��Щ�л��������ڲ����������ϵ�ֵ**
![�˴�����ͼƬ������](images/mysql-explain-ref.png)

**rows�����ݱ�ͳ����Ϣ������ѡ����������¹�����ҵ�����ļ�¼����Ҫ��ȡ������**
![�˴�����ͼƬ������](images/mysql-explain-rows-1.png)
��ͨ����������������641��
![�˴�����ͼƬ������](images/mysql-explain-rows-2.png)
������صĸ��������ٲ飬��Ҫ��ѯ�������ͱ�����

**Extra**
1. using filesort��mysql���޷�����������ɵ�������ʱ�������ʹ��һ���ⲿ���������򣬶����ǰ��ձ��ڵ�����˳����ж�ȡ��
![�˴�����ͼƬ������](images/mysql-explain-extra-using-filesort-1.png)
��������ʱ�ͻ�������Ƚ������򣬳���using filesortһ������Ϊorder by���������������ʧЧ����ý����Ż���
![�˴�����ͼƬ������](images/mysql-explain-extra-using-filesort-2.png)
order by��������ú�����������˳��͸���һ��
2. using temporary��ʹ������ʱ�������м�����mysql�ڶԲ�ѯ�������ʱʹ����ʱ��������������order by�ͷ����ѯgroup by��
![�˴�����ͼƬ������](images/mysql-explain-extra-using-temporary-1.png)
Ӱ���������Ҫô����������Ҫôgroup by��˳��Ҫ������һ��
![�˴�����ͼƬ������](images/mysql-explain-extra-using-temporary-2.png)
3. using index����ʾ��Ӧ��select������ʹ���˸�����������������˱��������У�Ч�ʺ�
��������������select���������ֻ����������ȡ�ã����ض�ȡ�����У��������������ĸ�������ѯ��С�ڵ���������������˳��һ�¡�
�����������Ҫ�ø�����������Ҫע��select����ֻȡ��Ҫ�õ����У�����select *��ͬʱ����������ֶ�һ���������ᵼ�������ļ��������ܻ��½���
![�˴�����ͼƬ������](images/mysql-explain-extra-using-index-1.png)
����using where����������������ִ��������ֵ�Ĳ���
![�˴�����ͼƬ������](images/mysql-explain-extra-using-index-2.png)
���û��ͬʱ����using where����������������ȡ���ݶ���ִ�в��Ҷ�����
4. using where������ʹ����where����
5. using join buffer��ʹ�������ӻ���
6. impossible where��where�Ӿ��ֵ��false
7. select tables optimized away
8. distinct���Ż�distinct���������ҵ���һƥ���Ԫ���ֹͣ��ͬ��ֵ�Ķ���

## InnoDB������
![�˴�����ͼƬ������](images/mysql-innodb-lock.png)

InnoDBĬ��ʹ��������ʵ�������ֱ�׼��������������������������

![�˴�����ͼƬ������](images/mysql-innodb-lock-s lock vs x lock.png)
ע�⣺
1. ������ʽ�������������������µļ���������������˹���Ԥ��
2. InnoDB���е������㷨���ǻ�������ʵ�ֵģ�������Ҳ�����������������䣻

### ��ǰ���Ϳ��ն�

 - **��ǰ����**������������ȡ��¼�����°汾���������֤���������������޸ĵ�ǰ��¼��ֱ����ȡ���������ͷ�����

ʹ�õ�ǰ���Ĳ�����Ҫ��������ʽ�����Ķ����������/����/ɾ����д������������ʾ��
```
select * from table where ? lock in share mode;
select * from table where ? for update;
insert into table values (��);
update table set ? where ?;
delete from table where ?;
```
- **���ն���**��������������ȡ��¼�Ŀ��հ汾�������°汾��ͨ��MVCCʵ�֣�
InnoDBĬ�ϵ�RR������뼶���£�����ʽ�ӡ�lock in share mode���롺for update���ġ�select�����������ڿ��ն�����֤����ִ�й�����ֻ�е�һ�ζ�֮ǰ�ύ���޸ĺ��Լ����޸Ŀɼ��������ľ����ɼ���

### MVCC
(ժ�ԡ�������Mysql��)
![�˴�����ͼƬ������](images/mysql-innodb-lock-mvcc.png)

������
ÿһ��������������ʱ�򣬶���һ��Ψһ�ĵ����İ汾�š� 
1. �ڲ������ʱ �� ��¼�Ĵ����汾�ž�������汾�š� 
�����Ҳ���һ����¼, ����id ������1 ����ô��¼���£�Ҳ����˵�������汾�ž�������汾�š�
id | name | create version | delete version
:--:|:--:|:--:|:--:
1  | test | 1 | 

2. �ڸ��²�����ʱ�򣬲��õ����ȱ�Ǿɵ����м�¼Ϊ��ɾ��������ɾ���汾��������汾�ţ�Ȼ�����һ���µļ�¼�ķ�ʽ�� 
���磬����������м�¼������IdΪ2 Ҫ��name�ֶθ���
```
update table set name= 'new_value' where id=1;
```
id | name | create version | delete version
:--:|:--:|:--:|:--:
1  | test | 1 | 2
1  | new_value | 2 |

3. ɾ��������ʱ�򣬾Ͱ�����汾����Ϊɾ���汾�š�����
```
    delete from table where id=1; 
```
id | name | create version | delete version
:--:|:--:|:--:|:--:
1  | new_value | 2 | 3

4. ��ѯ������ 
��������������Կ������ڲ�ѯʱҪ�����������������ļ�¼���ܱ������ѯ������ 

 - ɾ���汾�Ŵ��ڵ�ǰ����汾�ţ�����˵ɾ���������ڵ�ǰ��������֮�����ġ� 
 - �����汾��С�ڻ��ߵ��ڵ�ǰ����汾�� ������˵��¼�������������У����ڵ������������������֮ǰ��


### ���㷨
InnoDB��Ҫʵ�������������㷨��
![�˴�����ͼƬ������](images/mysql-innodb-lock-three-type-algorithms.png)

��ͬ��������뼶�𡢲�ͬ���������͡��Ƿ�Ϊ��ֵ��ѯ��ʹ�õ������㷨Ҳ��������ͬ���������InnoDBĬ�ϵ�RR���뼶�𡢵�ֵ��ѯΪ�������ܼ��������㷨��
![�˴�����ͼƬ������](images/mysql-innodb-lock-three-type-algorithms.png)

**ע�⣺��Ҫ�����µ�ֵ��ѯʹ�ø�������**
![�˴�����ͼƬ������](images/mysql-innodb-lock-equal-select-secondary-index.png)

**Gap��:** ��������������¼֮��ļ�϶���Ƿ�ֹ�ö��Ĺؼ������û����ͼ����ɫ��ʶ��Gap Lock���������������ڼ�϶�в�����һ����¼�磺��insert into stock (id,sku_id) values(2,103);�����ύ����ô�ڴ��������ظ�ִ����ͼ��SQL���ͻ��ѯ�����������²���ļ�¼�������ֻö������ö���ָ��ͬһ�����£�����ִ������ͬ����SQL�����ܵ��²�ͬ�Ľ�����ڶ��ε�SQL�����ܷ���֮ǰ�����ڵ��м�¼������Gap Lock�󣬲����������������ǰ���ȼ���϶���Ƿ��ѱ���������ֹ�ö��ĳ��֣�

### InnoDB��MyISAM������
����|�洢�ṹ|�洢�ռ�|����ֲ��|����֧��|��������|ȫ������|������|CURD����|���
:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:
MyISAM|�����塢�����ļ��������ļ�|�ɱ�ѹ�����洢�ռ�С|���ļ���ʽ�洢��������ֲ|��֧��|����|֧��|����û���κ������������ı�����|�ʺϴ���select����|��֧��
InnoDB|һ�������ļ�|�洢�ռ��|��Ҫ�������ݡ�����binlog|֧��|�����ͱ���|��֧��|�����Ǿ۴�����������������������Ҫ��|�ʺϴ���insert��update|֧��


## Q&A
### Limit����Ż�
1.�Ӳ�ѯ�Ż���
```
    select * from Member where MemberID >= (select MemberID from Member limit 100000,1) limit 100   
```
ȱ�㣺���ݱ����������ģ�����˵������where������where������ɸѡ���ݣ���������ʧȥ������

2.ʹ�� id �޶��Ż�
```
    select * from orders_history where id >= 1000001 limit 100;  
```
3.��������Ż���
��ƫ�Ƴ���һ���¼����ʱ��������������ƫ�ƾͷ�ת��
```
   �ܼ�¼����1,628,775
   ÿҳ��¼���� 40
   ��ҳ����1,628,775 / 40 = 40720
   �м�ҳ����40720 / 2 = 20360
   
��30000ҳ
   �������SQL:
   SELECT * FROM `abc` WHERE `BatchID` = 123 LIMIT 1199960, 40
   ʱ�䣺2.6493 ��
   
   �������sql:
   SELECT * FROM `abc` WHERE `BatchID` = 123 ORDER BY InputDate DESC LIMIT 428775, 40
   ʱ�䣺1.0035 ��
```
ȱ�㣺order by�Ż��Ƚ��鷳��Ҫ��������������Ӱ�����ݵ��޸�Ч�ʣ�����Ҫ֪���ܼ�¼����ƫ�ƴ������ݵ�һ��


## MySQL���Ӹ���
### ���Ӳ����Ҫ������
- ���⿪��binlog��־������log-bin������
- ����server-id��ͬ
- �ӿ����������ͨ����

### ԭ��
![�˴�����ͼƬ������](images/mysql-master-slave-replica-principle.jpg)

**�������̣�**
1. �ӿ����������̣߳�һ��I/O�̣߳�һ��SQL�̣߳�
2. i/o�߳�ȥ�������� ��binlog�������õ���binlog��־д��relay-log���м���־�� �ļ��У�
3. ���������һ�� log dump �̣߳��������ӿ� i/o�̴߳�binlog��
4. SQL �̣߳����ȡrelay-log�ļ��е���־���������ɾ����������ʵ�����ӵĲ���һ�£�����������һ�£�

### mysql���Ӹ��ƴ��ڵ����⣺
1. ����崻������ݿ��ܶ�ʧ
2. �ӿ�ֻ��һ��sql Thread������дѹ���󣬸��ƺܿ�����ʱ
 
**���������**
��ͬ������---������ݶ�ʧ������
���и���----����ӿ⸴���ӳٵ�����

### ��ͬ�����ƣ�
ȷ�������ύ��binlog���ٴ��䵽һ���ӿ⣬���ǲ���֤�ӿ�Ӧ������������binlog
![�˴�����ͼƬ������](images/mysql-semi-asyn.jpeg)

### ���и���
- ������ָ�ӿ�**���߳�**apply binlog
- �⼶����Ӧ��binlog��ͬһ�������ݸ��Ļ��Ǵ��е�(5.7�沢�и��ƻ���������)

�������ã�
```
    set global slave_parallel_workers=10;
```

### MySQL��д��������һ���Խ��
- **��ͬ������**
���Ӳ�һ�µ�ԭ������ʱ�����,����Ҫ���������ʱ��Ӱ�죬���Դ��������CUD����ʱ���й�ܣ��취���ǵ�����ͬ�����֮�������ϵ�д�����ٷ��أ����Ǵ�ҳ�˵�İ�ͬ������`semi-sync`��

 ![�˴�����ͼƬ������](images/mysql-semi-asyn-2.jpeg)

    �����ŵ㣺�������ݿ�ԭ�����ܣ��Ƚϼ�
    ����ȱ�㣺�����д����ʱ�ӻ��������������ή��

- ���ݿ��м��
 - CUD����
 ![�˴�����ͼƬ������](images/mysql-data-consistence-middleware-cud.jpg)
  - R����
 ![�˴�����ͼƬ������](images/mysql-data-consistence-middleware-r.jpg)

    �����ŵ㣺�ܱ�֤����һ��
    ����ȱ�㣺���ݿ��м���ĳɱ��Ƚϸ�

- **�����¼дkey��**
 - CUD����
    1. ��ĳ�����ϵ�ĳ��keyҪ����д��������¼��cache������á���������ͬ��ʱ�䡱��cache��ʱʱ�䣬����500ms
    2. �޸����ݿ�

 - R����
    1. �ȵ�cache��鿴����Ӧ��Ķ�Ӧkey��û��������� 
    2. ���cache hit����������ݣ�˵�����key�ϸշ�����д��������ʱ��Ҫ������·�ɵ���������µ����� 
    3. ���cache miss��˵�����key�Ͻ���û�з�����д��������ʱ������·�ɵ��ӿ⣬������д����

    �����ŵ㣺������ݿ��м�����ɱ��ϵ�
    ����ȱ�㣺����ȱ�㣺Ϊ�˱�֤��һ���ԡ���������һ��cache��������Ҷ�д���ݿ�ʱ������һ��cache����