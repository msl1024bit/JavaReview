# Java����

### HashMapԭ��(*)
- HashMap���ֻ����һ��Entry�ļ�ΪNull(�����Ḳ��)�����������Entry��ֵΪNull
- HashSet ��������� HashMap �Ļ�����ʵ�ֵ�
- ����������Խ����ô�Կռ�����ø���֣�������Ч�ʵ�Ҳ��Խ�ͣ�����������ԽС����ô��ϣ������ݽ�Խϡ�裬�Կռ���ɵ��˷�Ҳ��Խ���ء�ϵͳĬ�ϸ�������0.75
- ����put������ֵʱ��HashMap���Ȼ����Key��hashCode������Ȼ����ڴ˻�ȡKey��ϣ�룬ͨ����ϣ������ҵ�ĳ��Ͱ�����λ�ÿ��Ա���֮ΪbucketIndex.������������hashCode��ͬ����ôequalsһ��Ϊfalse�����������hashCode��ͬ��equalsҲ��һ��Ϊtrue�����ԣ������ϣ�hashCode���ܴ�����ײ�����������ײ����ʱ����ʱ��ȡ��bucketIndexͰ���Ѵ洢��Ԫ�أ���ͨ��hashCode()��equals()������Ƚ����ж�Key�Ƿ��Ѵ��ڡ�����Ѵ��ڣ���ʹ����Valueֵ�滻��Valueֵ�������ؾ�Valueֵ����������ڣ������µļ�ֵ�Ե�Ͱ�С���ˣ��� HashMap�У�equals() ����ֻ���ڹ�ϣ����ײʱ�Żᱻ�õ���
- ���ȣ��ж�key�Ƿ�Ϊnull����Ϊnull����ֱ�ӵ���putForNullKey����������Ϊ�գ����ȼ���key��hashֵ��Ȼ�����hashֵ������table�����е�����λ�ã����table�����ڸ�λ�ô���Ԫ�أ�������Ƿ������ͬ��key���������򸲸�ԭ��key��value�����򽫸�Ԫ�ر�������ͷ�����ȱ����Ԫ�ط�����β�������⣬��table�ڸô�û��Ԫ�أ���ֱ�ӱ��档
- HashMap ��Զ����������ı�ͷ�����Ԫ�ء�


----------


### hash()����
```
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

- hash() �������ڶ�Key��hashCode�������¼��㣬�������������hashֵ��hashMap��(length-1)����&����󣬻�õ�λ������[0��length-1]��һ��ֵ���ر�أ����ֵ�ֲ���Խ���ȣ�HashMap �Ŀռ�������Ҳ��Խ�ߣ���ȡЧ��Ҳ��Խ�ã���֤Ԫ�ؾ��ȷֲ���table��ÿ��Ͱ���Ա������ÿռ䡣
- hash():ʹ��hash()������һ�������hashCode�������¼�����Ϊ�˷�ֹ�������µ�hashCode()����ʵ�֡�����hashMap��֧�����鳤������2���ݴΣ�ͨ�����ƿ���ʹ��λ�����ݾ����Ĳ�ͬ���Ӷ�ʹhashֵ�ķֲ��������ȡ�
- ��֤Ԫ�ؾ��ȷֲ���table��ÿ��Ͱ��; ��lengthΪ2��n�η�ʱ��h&(length -1)���൱�ڶ�h%length�����������hash��ײ�������ٶȱ�ֱ��ȡģҪ��ö࣬�ռ�������Ҳ����ߣ�����HashMap���ٶ��ϵ�һ���Ż���

### resize()����
- ����������HashMap��ʱ�򣬲���Ҫ��JDK1.7��ʵ���������¼���hash��ֻ��Ҫ����ԭ����hashֵ�������Ǹ�bit��1����0�ͺ��ˣ���0�Ļ�����û�䣬��1�Ļ��������"ԭ����+oldCap"

## ConcurrenytHashMapԭ��(*)
### JDK1.7ͼʾ��
![�˴�����ͼƬ������](images/collection-concurrent-hashmap-jdk1.7.jpg)


### JDK1.8ͼʾ��
  ![�˴�����ͼƬ������](images/collection-concurrent-hashmap-jdk1.8.jpg)


- 1.7���õķֶ�������1.8���õ���CAS��synchronize����
- 1.8��ס����table���𣬶�1.7��ס��segment���������ȸ�ϸ���������ܸ���
- ����1.8������ס����table������ʱ��Ӱ�������̵߳Ķ�д���������Ż��������߳�һ��������ݣ��ӿ������ٶ�
- �������ݡ�����ForwardingNode�࣬��ı�sizeCtl���ֵ

### HashMap�̲߳���ȫ
- ��HashMap���������ع�ϣʱ����Entry���γɻ���һ��Entry�����л����Ʊػᵼ����ͬһ��Ͱ�н��в��롢��ѯ��ɾ���Ȳ���ʱ������ѭ����

### Segment����
- Segment ��̳��� ReentrantLock �࣬�Ӷ�ʹ�� Segment �����ܳ䵱���Ľ�ɫ
- ��Segment���У�count ������һ��������������ʾÿ�� Segment �������� table ��������� HashEntry ����ĸ�����Ҳ���� Segment �а����� HashEntry ������������ر���Ҫע����ǣ�֮������ÿ�� Segment �����а���һ������������������ ConcurrentHashMap ��ʹ��ȫ�ֵļ��������Ƕ� ConcurrentHashMap �����ԵĿ��ǣ���Ϊ��������Ҫ���¼�����ʱ��������������ConcurrentHashMap��

## ArrayList
- ���޲������췽������ ArrayList ʱ��ʵ���ϳ�ʼ����ֵ����һ�������顣������������������Ԫ�ز���ʱ������������������������������ӵ�һ��Ԫ��ʱ������������Ϊ10

### ����
```
    /�ж��Ƿ���Ҫ����
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            //����grow�����������ݣ����ô˷��������Ѿ���ʼ������
            grow(minCapacity);
    }
    
    /**
     * Ҫ�������������С
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * ArrayList���ݵĺ��ķ�����
     */
    private void grow(int minCapacity) {
        // oldCapacityΪ��������newCapacityΪ������
        int oldCapacity = elementData.length;
        //��oldCapacity ����һλ����Ч���൱��oldCapacity /2��
        //����֪��λ������ٶ�ԶԶ�����������㣬��������ʽ�Ľ�����ǽ�����������Ϊ��������1.5����
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //Ȼ�����������Ƿ������С��Ҫ������������С����С��Ҫ��������ô�Ͱ���С��Ҫ���������������������
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
       // ������������� MAX_ARRAY_SIZE,����(ִ��) `hugeCapacity()` �������Ƚ� minCapacity �� MAX_ARRAY_SIZE��
       //���minCapacity�����������������������Ϊ`Integer.MAX_VALUE`��������������С��Ϊ MAX_ARRAY_SIZE ��Ϊ `Integer.MAX_VALUE - 8`��
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```
    

- ***int newCapacity = oldCapacity + (oldCapacity >> 1),���� ArrayList ÿ������֮�����������Ϊԭ���� 1.5 ����*** 
- ArrayList Դ������һ�� ensureCapacity ������������� ArrayList �ڲ�û�б����ù������Ժ���Ȼ���ṩ���û����õģ������ add ����Ԫ��֮ǰ�� ensureCapacity �������Լ����������·���Ĵ���

## LinkedList
**�ڲ��ṹ**
![�˴�����ͼƬ������](images/collection-linked-list-structure.jpeg)

- LinkedList��һ��ʵ����List�ӿں�Deque�ӿڵ�˫������ 
- LinkedList�ײ������ṹʹ��֧�ָ�Ч�Ĳ����ɾ��������������ʵ����Deque�ӿڣ�ʹ��LinkedList��Ҳ���ж��е�����; 
- LinkedList�����̰߳�ȫ�ģ������ʹLinkedList����̰߳�ȫ�ģ����Ե��þ�̬��Collections���е�synchronizedList������


## Q&A
1. List�������ص�
- ArrayList:
�ײ����ݽṹ�����飬��ѯ�죬��ɾ��
�̲߳���ȫ��Ч�ʸ�
- Vector:
�ײ����ݽṹ�����飬��ѯ�죬��ɾ��
�̰߳�ȫ��Ч�ʵ�
- LinkedList:
�ײ����ݽṹ��˫��������ѯ������ɾ��
�̲߳���ȫ��Ч�ʸ�