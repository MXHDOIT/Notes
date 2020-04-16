# MAC地址表、ARP缓存表以及路由表

## 一.MAC地址表

一：MAC地址表详解

　　说到MAC地址表，就不得不说一下交换机的工作原理了，因为交换机是根据MAC地址表转发数据帧的。在交换机中有一张记录着[局域网](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%BE%D6%D3%F2%CD%F8&k0=%BE%D6%D3%F2%CD%F8&kdi0=0&luki=2&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)主机MAC地址与交换机接口的对应关系的表，交换机就是根据这张表负责将数据帧传输到指定的主机上的。

　　交换机的工作原理

　　交换机在接收到数据帧以后，首先、会记录数据帧中的源MAC地址和对应的接口到MAC表中，接着、会检查自己的MAC表中是否有数据帧中目标MAC地址的信息，如果有则会根据MAC表中记录的对应接口将数据帧发送出去(也就是单播)，如果没有，则会将该数据帧从非接受接口发送出去(也就是广播)。

　　如下图：详细讲解交换机传输数据帧的过程

　![image-20200413211322037](E:\GitHub\picBed\img\image-20200413211322037.png)

　　1)主机A会将一个源MAC地址为自己，目标MAC地址为主机B的数据帧发送给交换机。

　　2)交换机收到此数据帧后，首先将数据帧中的源MAC地址和对应的接口(接口为f 0/1) 记录到MAC地址表中。

　　3)然后交换机会检查自己的MAC地址表中是否有数据帧中的目标MAC地址的信息，如果有，则从MAC地址表中记录的接口发送出去，如果没有，则会将此数据帧从非接收接口的所有接口发送出去(也就是除了f 0/1接口)。

　　4)这时，局域网的所有主机都会收到此数据帧，但是只有主机B收到此数据帧时会响应这个广播，并回应一个数据帧，此数据帧中包括主机B的MAC地址。

　　5)当交换机收到主机B回应的数据帧后，也会记录数据帧中的源MAC地址(也就是主机B的MAC地址)，这时，再当主机A和主机B通信时，交换机根据MAC地址表中的记录，实现单播了。

　　如下图：当[局域网](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%BE%D6%D3%F2%CD%F8&k0=%BE%D6%D3%F2%CD%F8&kdi0=0&luki=2&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)存在多个交换机互联的时候，交换机的MAC地址表是怎么记录的呢？

　![image-20200413211253767](E:\GitHub\picBed\img\image-20200413211253767.png)

　　1)主机A将一个源MAC地址为自己，目标MAC地址主机C的数据帧发送给交换机

　　2)交换机1收到此数据帧后，会学习源MAC地址，并检查MAC地址表，发现没有目标MAC地址的记录，则会将数据帧广播出去，主机B和交换机2都会收到此数据帧。

　　3)[交换机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%BD%BB%BB%BB%BB%FA&k0=%BD%BB%BB%BB%BB%FA&kdi0=0&luki=7&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)2收到此数据帧后也会将数据帧中的源MAC地址和对应的接口记录到MAC地址表中，并检查自己的MAC地址表，发现没有目标MAC地址的记录，则会广播此数据帧。

　　4)主机C收到数据帧后，会响应这个数据帧，并回复一个源MAC地址为自己的数据帧，这时交换机1和交换机1都会将主机C的MAC地址记录到自己的MAC地址表中，并且以单播的形式将此数据帧发送给主机A。

　　5)这时，主机A和主机C通信就是一单播的形式传输数据帧了，主机B和主机C通信如上述过程一样，因此交换机2的MAC地址表中记录着主机A和[主机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%D6%F7%BB%FA&k0=%D6%F7%BB%FA&kdi0=0&luki=3&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)B的MAC地址都对应接口f 0/1。

　　总结：从上面的两幅图可以看出，交换机具有动态学习源MAC地址的功能，并且交换机的一个接口可以对应多个MAC地址，但是一个MAC地址只能对应一个接口。

　　注意：交换机动态学习的MAC地址默认只有300S的有效期，如果300S内记录的MAC地址没有通信，则会删除此记录。

<hr />

## 二.ARP缓存表

​	上面我们讲解了[交换机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%BD%BB%BB%BB%BB%FA&k0=%BD%BB%BB%BB%BB%FA&kdi0=0&luki=7&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)的工作原理，知道交换机是通过MAC地址通信的，但是我们是如何获得目标主机的MAC地址呢？这时我们就需要使用ARP协议了，在每台主机中都有一张ARP表，它记录着主机的IP地址和MAC地址的对应关系。

　　ARP协议：ARP协议是工作在网络层的协议，它负责将IP地址解析为MAC地址。

　　如下图：详细讲解ARP的工作原理。

　![image-20200413211239901](E:\GitHub\picBed\img\image-20200413211239901.png)

　　1)如果主机A想发送数据给主机B，主机A首先会检查自己的ARP缓存表，查看是否有主机B的IP地址和MAC地址的对应关系，如果有，则会将主机B的MAC地址作为源MAC地址封装到数据帧中。如果没有，主机A则会发送一个ARP请求信息，请求的目标IP地址是主机B的IP地址，目标MAC地址是MAC地址的广播帧(即FF-FF-FF-FF-FF-FF)，源IP地址和MAC地址是[主机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%D6%F7%BB%FA&k0=%D6%F7%BB%FA&kdi0=0&luki=3&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)A的IP地址和MAC地址。

　　2)当交换机接受到此数据帧之后，发现此数据帧是广播帧，因此，会将此数据帧从非接收的所有接口发送出去。

　　3）当主机B接受到此数据帧后，会校对IP地址是否是自己的，并将主机A的IP地址和MAC地址的对应关系记录到自己的ARP缓存表中，同时会发送一个ARP应答，其中包括自己的MAC地址。

　　4)[主机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%D6%F7%BB%FA&k0=%D6%F7%BB%FA&kdi0=0&luki=3&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)A在收到这个回应的数据帧之后，在自己的ARP缓存表中记录主机B的IP地址和MAC地址的对应关系。而此时[交换机](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%BD%BB%BB%BB%BB%FA&k0=%BD%BB%BB%BB%BB%FA&kdi0=0&luki=7&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)已经学习到了主机A和主机B的MAC地址了。

<hr />

## 三. 路由表

　路由器负责不同网络之间的通信，它是当今网络中的重要设备，可以说没有路由器就没有当今的互联网。在路由器中也有一张表，这张表叫路由表，记录着到不同网段的信息。路由表中的信息分为直连路由和非直连路由。

　　直连路由：是直接连接在路由器接口的网段，由路由器自动生成。

　　非直连路由：就是不是直接连接在[路由器](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9%C6%F7&k0=%C2%B7%D3%C9%C6%F7&kdi0=0&luki=6&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)接口上的网段，此记录需要手动添加或者是使用动态路由。

　　路由表中记录的条目有的需要手动添加(称为静态路由)，有的测试动态获取的(称为动态路由)。直连路由属于静态路由。

　　路由器是工作在网络层的，在网络层可以识别逻辑地址。当路由器的某个接口收到一个包时，路由器会读取包中相应的目标的逻辑地址的网络部分，然后在路由表中进行查找。如果在路由表中找到目标地址的路由条目，则把包转发到路由器的相应接口，如果在路由表中没有找到目标地址的路由条目，那么，如果路由配置默认路由，就科举默认路由的配置转发到路由器的相应接口；如果没有配置默认路由，则将该包丢弃，并返回不可到达的信息。这就是数据[路由](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9&k0=%C2%B7%D3%C9&kdi0=0&luki=4&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)的过程。

　　如下图：详细介绍路由器的工作原理

　![image-20200413211213915](E:\GitHub\picBed\img\image-20200413211213915.png)

　　1)HostA在网络层将来自上层的报文封装成IP数据包，其中源IP地址为自己，目标IP地址是HostB，HostA会用本机配置的24位[子网掩码](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%D7%D3%CD%F8%D1%DA%C2%EB&k0=%D7%D3%CD%F8%D1%DA%C2%EB&kdi0=0&luki=1&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)与目标地址进行“与”运算，得出目标地址与本机不是同一网段，因此发送HostB的数据包需要经过网关路由A的转发。

　　2)HostA通过ARP请求获取网关[路由](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9&k0=%C2%B7%D3%C9&kdi0=0&luki=4&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)A的E0口的MAC地址，并在链路层将路由器E0接口的MAC地址封装成目标MAC地址，源MAC地址是自己。

　　3)路由器A从E0可接收到数据帧，把数据链路层的封装去掉，并检查路由表中是否有目标IP地址网段(即192.168.2.2的网段)相匹配的的项，根据路由表中记录到192.168.2.0网段的数据请发送给下一跳地址10.1.1.2，因此数据在路由器A的E1口重新封装，此时，源MAC地址是路由器A的E1接口的MAC地址，封装的目标MAC地址则是[路由器](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9%C6%F7&k0=%C2%B7%D3%C9%C6%F7&kdi0=0&luki=6&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)2的E1接口的MAC地址。

　　4)路由B从E1口接收到数据帧，同样会把数据链路层的封装去掉，对目标IP地址进行检测，并与路由表进行匹配，此时发现目标地址的网段正好是自己E0口的直连网段，路由器B通过ARP广播，获知HostB的MAC地址，此时数据包在路由器B的E0接口再次封装，源MAC地址是路由器B的E0接口的MAC地址，目标MAC地址是HostB的MAC地址。封装完成后直接从路由器的E0接口发送给HostB。

　　5)此时HostB才会收到来自HostA发送的数据。

　　总结：[路由](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9&k0=%C2%B7%D3%C9&kdi0=0&luki=4&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)表负责记录一个网络到另一个网络的路径，因此[路由器](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=1001&ch=0&di=128&fv=19&is_app=0&jk=9c8c6a7fafd6e446&k=%C2%B7%D3%C9%C6%F7&k0=%C2%B7%D3%C9%C6%F7&kdi0=0&luki=6&n=10&p=baidu&q=67051059_cpr&rb=0&rs=1&seller_id=1&sid=46e4d6af7f6a8c9c&ssp2=1&stid=0&t=tpclicked3_hc&td=1740074&tu=u1740074&u=http%3A%2F%2Fwww%2Eeducity%2Ecn%2Fnet%2F1284034%2Ehtml&urlid=0)是根据路由表工作的。

　　



文章出自：http://www.educity.cn/net/1284034.html 