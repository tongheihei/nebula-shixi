# 不涉及zone

## 1.1 三副本

集群 host status

![image-20210701104542794](/Users/taolei/Library/Application Support/typora-user-images/image-20210701104542794.png)

### 1.1.1 新增机器

新增机器后 **host status**

![image-20210701104559378](/Users/taolei/Library/Application Support/typora-user-images/image-20210701104559378.png)

执行**balance data,balance leader** 后的 **host status**

![image-20210701113810108](/Users/taolei/Library/Application Support/typora-user-images/image-20210701113810108.png)

##### 测试结果

无异常，Balance data 正常执行，300个含有对应数据的分片被均匀的分散到各个节点当中，100个partition的leader也均匀的分散到各个节点当中。

### 1.1.2 剔除机器

#### 1.1.2.1 使用balance remove

**Host status**

![image-20210701110251816](/Users/taolei/Library/Application Support/typora-user-images/image-20210701110251816.png)

执行命令

```
balance data remove 192.168.8.5:7857;
```

执行完后的**host status**

![image-20210701114159791](/Users/taolei/Library/Application Support/typora-user-images/image-20210701114159791.png)

##### 测试结果

无异常，**balance data remove** 命令执行过后，分片均匀的分布在各个节点当中，leader也可以通过执行balance leader使其均匀的分布在各个节点当中。

####  1.1.2.2 机器直接离线后balance

机器离线前集群data,leader分布情况：

![image-20210701115621259](/Users/taolei/Library/Application Support/typora-user-images/image-20210701115621259.png)

机器(**192.168.8.5:7857**)离线后集群data,leader分布情况：

![image-20210701115801304](/Users/taolei/Library/Application Support/typora-user-images/image-20210701115801304.png)

##### 测试结果

无异常。在机器直接离线过后，由于是三副本，database含有其他两个副本的数据，因而就算此副本离线，balance data也能把该分片数据给拷贝到其他节点。





### 1.1.3 替换机器

描述：加一台新机器，同时剔除一台老机器，机器数不变


- 目前的data,leader分布情况
![image-20210701121114044](/Users/taolei/Library/Application Support/typora-user-images/image-20210701121114044.png)

- 新增一台新机器,**balance data ,balance leader** 过后data,leader分布情况

![image-20210701121904896](/Users/taolei/Library/Application Support/typora-user-images/image-20210701121904896.png)

- 剔除一台老机器后,data,leader分布情况

  剔除执行：

  - 命令

  ```bash
  balance data remove 192.168.8.5:7557;
  ```

  - 结果

![image-20210701122027842](/Users/taolei/Library/Application Support/typora-user-images/image-20210701122027842.png)

  执行完**balance data,balance leader**后，**Data, leader**分布情况：

![image-20210701132536372](/Users/taolei/Library/Application Support/typora-user-images/image-20210701132536372.png)

#### 测试结果

无异常。增加一台机器过后，执行balance任务，分片会均匀的分布在目前在线的机器中。再将一台机器balance remove掉，分片会均匀的分布在剩余的在线的机器当中。

## 1.2 单副本

集群host status 

![image-20210701145726276](/Users/taolei/Library/Application Support/typora-user-images/image-20210701145726276.png)

### 1.2.1 新增机器

新增机器后host status

![image-20210701150018751](/Users/taolei/Library/Application Support/typora-user-images/image-20210701150018751.png)

执行balance data ,balance leader过后的host status

![image-20210701150354519](/Users/taolei/Library/Application Support/typora-user-images/image-20210701150354519.png)

##### 测试结果

无异常，100个单副本的part被均匀的分散到各个节点中。同时leader也被均匀的分散到各个节点当中。

### 1.2.2 剔除机器
#### 1.2.2.1 使用balance remove

**Host status**

![image-20210701150354519](/Users/taolei/Library/Application Support/typora-user-images/image-20210701150354519.png)

执行命令

```bash
balance data remove 192.168.8.5:7957;
```

执行完后的**host status**

![image-20210701151452457](/Users/taolei/Library/Application Support/typora-user-images/image-20210701151452457.png)

##### 测试结果

无异常，**balance data remove** 命令执行过后，分片均匀的分布在各个节点当中，对于leader来说，leader也可以通过执行balance leader使其均匀的分布在各个节点当中。

####  1.2.2.2 机器直接离线后balance

机器离线前集群data,leader分布情况：

![image-20210701170845500](/Users/taolei/Library/Application Support/typora-user-images/image-20210701170845500.png)

机器(**192.168.8.5:7857**)离线后集群data,leader分布情况：

![image-20210701170634942](/Users/taolei/Library/Application Support/typora-user-images/image-20210701170634942.png)

**192.168.8.5:7857**机器离线,执行**balance data, balance leader**结果



![image-20210701171004144](/Users/taolei/Library/Application Support/typora-user-images/image-20210701171004144.png)

##### 测试结果

无异常。在机器直接离线过后，由于是单副本，space会丢失1/4的数据，因而在balance data的时候会失败，属于正常的现象。



### 1.2.3 替换机器

描述：加一台新机器，同时剔除一台老机器，机器数不变


- 目前的data,leader分布情况
  ![image-20210701163330522](/Users/taolei/Library/Application Support/typora-user-images/image-20210701163330522.png)

- 新增一台新机器,**balance data ,balance leader** 过后data,leader分布情况

![image-20210701164048101](/Users/taolei/Library/Application Support/typora-user-images/image-20210701164048101.png)

- 剔除一台老机器后,data,leader分布情况

  剔除执行：

  - 命令

  ```bash
  balance data remove 192.168.8.5:7557;
  ```

  - 结果

![image-20210701165047528](/Users/taolei/Library/Application Support/typora-user-images/image-20210701165047528.png)

  执行完**balance data,balance leader**剔除相关节点后，**Data, leader**分布情况：

![image-20210701165101192](/Users/taolei/Library/Application Support/typora-user-images/image-20210701165101192.png)

#### 测试结果

无异常。增加一台机器过后，执行balance任务，分片会均匀的分布在目前在线的机器中。再将一台机器balance remove掉，分片会均匀的分布在剩余的在线的机器当中。

## 