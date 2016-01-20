# DataAcquisitionXiDian

---

A project created for XiDianUniversity

and VIs contained in project have been protected by password

and email me: dgt_x@foxmail.com  to get it!

All rights reserved!

---

###写一写关于LabVIEW使用数据库的那些事

首先呢，LabVIEW开发系统自带有与**数据库进行连接**的库，注意是连接，因为直接通过LabVIEW是无法创建数据库的，LabVIEW只自带了和数据库进行连接的库。但是通过ActiveX是可以创建数据库的。

###1. 具体介绍一下如何通过ActiveX(自动化引用句柄)来创建数据库：

自动化引用句柄－与ActiveX函数配合使用。当打开一个指向ActiveX服务器对象的引用，且需要将引用作为参数传递给另一个VI时，可使用该类引用句柄控件。如需指定引用句柄的类型，可右键单击控件，从快捷菜单中选择选择ActiveX类»浏览，打开从类型库中选择对象对话框，浏览可用的ActiveX服务器应用程序。

首先在前面板上点击右键，在引用句柄下选择自动化引用句柄，放置在前面板上，在建立的图标上右击，点击选择ActiveX类，自定义，进入选择对话框。如图所示：

![选择ActiveX类](http://7xq6lu.com1.z0.glb.clouddn.com/activex2.png "选择ActiveX类")

选择如图所示的ADOX._Catalog后，转入程序框图，编写程序如图所示：

![程序框图](http://7xq6lu.com1.z0.glb.clouddn.com/activex1.png "程序框图")

该程序最关键的一步是设置合理的ConnectString，这个ConnectString是代表数据库的连接信息。这个连接信息是使用LabVIEW自带库函数操作数据库最关键的一个信息了，因此集中放在下一节解释。

###2. 如何使用LabVIEW自带的Database库函数操作数据库

连接并使用数据库的一个基本流程就是，open connection->Execute Query->Fetch Recordset Data->Free Object->Close connection.

如图所示：

![流程](http://7xq6lu.com1.z0.glb.clouddn.com/1.png "流程")

其中open connection函数涉及到连接数据库一个关键的内容就是设置合理的连接字符串，连接字符串包括Provider和Data Source，Provider一般是与连接的数据库属性有关的，在这里我使用的是Microsoft的Acess因此连接字符串的Provider是：Provider=Microsoft.JET.OLEDB.4.0;然后DataSource是你将要连接的数据库地址。将这两个字符串连接起来，传入open connection函数中的connection information端子上。

Execute Query函数接受一个SQL语句的字符串，用于进行数据库操作。SQL语句语法等内容请移步：[SQL语句](http://www.w3school.com.cn/sql/ "SQL语句")

其中Fetch Recordset是用于读取数据的，这里面有另外一个核心技术，就是将数据库查询出来的变体数据转换至字符串数据，下面将会介绍。

最后是Free Object和Close connection函数，也就是释放对象并且关闭连接。在这里我曾经遇到一个问题，利用for循环进行了多次的Execute Query之后调用Free Object和Close Connection会输出504错误，Open recordset object，由于vi也能正常实现功能，所以这个也不能完全算是错误，但是如果错误随着错误流输出的话会影响以后程序的运行，因此在Close connection函数后面加一个清楚错误函数，将该504错误清除。

