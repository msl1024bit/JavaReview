## Reactor�߳�ģ��
### Reactor���߳�
Reactor���߳�ģ�ͣ�ָ�������е�IO��������ͬһ��NIO�߳��������
![�˴�����ͼƬ������](images/netty-single-thread-reactor.png)

### Rector���߳�ģ��
Rector���߳�ģ���뵥�߳�ģ���������������һ��NIO�̴߳���IO����������ԭ��ͼ���£�
![�˴�����ͼƬ������](images/netty-multi-thread-reactor.png)

### ����Reactor�߳�ģ��
��������ڽ��տͻ������ӵĲ����Ǹ�1��������NIO�̣߳�����һ��������NIO�̳߳ء�
![�˴�����ͼƬ������](images/netty-master-slave-reactor.png)

### Netty�߳�ģ��
ͬʱ֧���������֡�
![�˴�����ͼƬ������](images/netty-thread-model.png)

## Q&A
1. Ϊʲôѡ��netty��
 - APIʹ�ü򵥣������ż��͡�
 - ����ǿ��Ԥ���˶��ֱ���빦�ܣ�֧�ֶ���Э�鿪����
 - ��������ǿ������ͨ��ChannelHadler������չ��
 - ���ܸߣ��Ա�����NIO��ܣ�Netty�ۺ��������š�
 - �����˴��ģ��Ӧ����֤���ڻ������������ݡ�������Ϸ����ҵӦ�á���������õ��ɹ����ܶ������Ŀ��ͨ�ŵײ������Netty������Dubbo
 - �ȶ����޸���NIO���ֵ�����Bug��
2. ԭ���� NIO �� JDK 1.7 �汾���� epoll bug��
![�˴�����ͼƬ������](images/nio-epoll-bug.png)
3. ʲô��TCP ճ��/����Լ����������
**ԭ��**
 - Ҫ���͵����ݴ���TCP���ͻ�����ʣ��ռ��С�����ᷢ�������
 - ���������ݴ���MSS������ĳ��ȣ���TCP�ڴ���ǰ�����в����
 - Ҫ���͵�����С��TCP���ͻ������Ĵ�С��TCP�����д�뻺����������һ�η��ͳ�ȥ�����ᷢ��ճ����
 - �������ݶ˵�Ӧ�ò�û�м�ʱ��ȡ���ջ������е����ݣ�������ճ����


 **���������**
 - ��Ϣͷ���������ݰ��ĳ���
 - ���Ͷ˽�ÿ�����ݰ���װΪ�̶����ȣ������Ŀ���ͨ����0��䣩
 - ���ݰ�֮�����÷ָ���

 **Netty�������**
 - LineBasedFrameDecoder:�Ի��з�Ϊ������־�Ľ�������֧��Я�����������߲�Я�����������ֽ��뷽ʽ��ͬʱ֧�����õ��е���󳤶�
 - FixedLengthFrameDecoder:�̶�����
 - DelimiterBasedFrameDecoder���̶��ַ��з�
 
4. Netty���㿽��
 - DirectByteBufͨ��ֱ���ڶ�������ڴ�ķ�ʽ�����������ݴӶ��ڿ���������Ĺ���
 - ͨ�����ByteBuf�ࣺ��CompositeByteBuf�������ByteBuf�ϲ�Ϊһ���߼��ϵ�ByteBuf
 - ͨ�����ְ�װ����, �� byte[]��ByteBuf��ByteBuffer�Ȱ�װ��һ��ByteBuf����
 - ͨ��slice����, ��һ��ByteBuf�ֽ�Ϊ�������ͬһ���洢�����ByteBuf, �������ڴ�Ŀ�����������Ҫ���в������ʱ�ǳ�����
 - ͨ��FileRegion��װ��FileChannel.tranferTo���������ļ�����ʱ, ����ֱ�ӽ��ļ������������ݷ��͵�Ŀ��Channel, ������ͨ��ѭ��write��ʽ���µ��ڴ濽�����������ַ�ʽ����Ҫ�õ�����ϵͳ���㿽����֧�ֵģ����netty�����еĲ���ϵͳ��֧���㿽�������ԣ���netty��Ȼ�޷������㿽��

5. Netty �ڲ�ִ������
 - ����ServerBootStrapʵ��
 - ���ò���Reactor�̳߳أ�EventLoopGroup��EventLoop���Ǵ�������ע�ᵽ���̵߳�Selector�����Channel
 - ���ò��󶨷���˵�channel
 - �������������¼���ChannelPipeline��handler������ʱ����������ʽ��������ת��handler��ɶ����Ĺ��ܶ��ƣ��������� SSl��ȫ��֤
 - �󶨲����������˿ڣ�**����ǿͻ��˴˲����첽����TCP����**��
 - ����ѵ��׼��������channel����Reactor�̣߳�NioEventLoopִ��pipline�еķ��������յ��Ȳ�ִ��channelHandler 

6. Netty ����ʵ��
 - *�������*
 Netty���ṩ��һ��IdleStateHandler�������������
    ```
    ch.pipeline().addLast("ping", new IdleStateHandler(60, 20, 60 * 10, TimeUnit.SECONDS));
    ```
    �ڴ������ݵ�ClientPoHandlerProto������userEventTriggered�����������������,event.state()��״̬�ֱ��Ӧ��������������ʱ�����ã�������ĳ��ʱ�������ʱ�ᴥ���¼���
���ͻ���20��û������˷��͹����ݣ��ͻᴥ��`IdleState.WRITER_IDLE`�¼������ʱ�����Ǿ������˷���һ���������ݣ���ҵ���޹أ�ֻ��������������յ�����֮��ͻ�ظ�һ����Ϣ����ʾ�Ѿ��յ�����������Ϣ��ֻҪ�յ��˷���˻ظ�����Ϣ����ô�Ͳ��ᴥ��`IdleState.READER_IDLE`�¼������������`IdleState.READER_IDLE`�¼���˵�������û�и��ͻ�����Ӧ�����ʱ�����ѡ���������ӡ�

 - ����ʱ��������
 ���Ӹ��������ļ�����ConnectionListener,����ӵ�ChannelFuture��ȥ����ʹ�ã�����ʧ�ܵ�ʱ������ConnectionListener�е�operationComplete����ִ�����ǵ������߼�
 ```
 public class ConnectionListener implements ChannelFutureListener {

    private ImConnection imConnection = new ImConnection();

    @Override
    public void operationComplete(ChannelFuture channelFuture) throws Exception {
        if (!channelFuture.isSuccess()) {
            final EventLoop loop = channelFuture.channel().eventLoop();
            loop.schedule(new Runnable() {
                @Override
                public void run() {
                    System.err.println("��������Ӳ��ϣ���ʼ��������...");
                    imConnection.connect(ImClientApp.HOST, ImClientApp.PORT);
                }
            }, 1L, TimeUnit.SECONDS);
        } else {
            System.err.println("��������ӳɹ�...");
        }
    }
}
 ```
 
     ```
         public class ImConnection {
        
            private Channel channel;
        
            public Channel connect(String host, int port) {
                doConnect(host, port);
                return this.channel;
            }
        
            private void doConnect(String host, int port) {
                EventLoopGroup workerGroup = new NioEventLoopGroup();
                try {
                    Bootstrap b = new Bootstrap();
                    b.group(workerGroup);
                    b.channel(NioSocketChannel.class);
                    b.option(ChannelOption.SO_KEEPALIVE, true);
                    b.handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        public void initChannel(SocketChannel ch) throws Exception {
        
                            // ʵ���ഫ�����ݣ�protobuf���л�
                            ch.pipeline().addLast("decoder",  
                                    new ProtobufDecoder(MessageProto.Message.getDefaultInstance()));  
                            ch.pipeline().addLast("encoder",  
                                    new ProtobufEncoder());  
                            ch.pipeline().addLast(new ClientPoHandlerProto());
        
                        }
                    });
        
                    ChannelFuture f = b.connect(host, port);
                    f.addListener(new ConnectionListener());
                    channel = f.channel();
                } catch(Exception e) {
                    e.printStackTrace();
                }
            }
        
        }
     ```
   
   - ���������ӶϿ�ʱ����:
    �����ӶϿ�ʱ���ᴥ�� channelInactive ����, �����������߼��������һ����
       ```
    @Override
    public void channelInactive(ChannelHandlerContext ctx) throws Exception {
        System.err.println("������...");
        //ʹ�ù����ж�������
        final EventLoop eventLoop = ctx.channel().eventLoop();
        eventLoop.schedule(new Runnable() {
            @Override
            public void run() {
                imConnection.connect(ImClientApp.HOST, ImClientApp.PORT);
            }
        }, 1L, TimeUnit.SECONDS);
        super.channelInactive(ctx);
    }
    ```
   