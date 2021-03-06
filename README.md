
## centos安装ssdb中碰到了官网没有给出的错误，以下是解决办法。（有其他问题欢迎大家一起沟通解决，有其他问题会后续更新此文档）    

#### 1、centos安装ssdb，依次执行以下命令：

   1.1: wget --no-check-certificate https://github.com/ideawu/ssdb/archive/master.zip    
   1.2: unzip master    
   1.3: cd ssdb-master    
   1.4: make    
   如果报以下错误：    
   ERROR! autoconf required! install autoconf first    

   Makefile:4: build_config.mk: No such file or directory    
   make: *** No rule to make target `build_config.mk'.  Stop. 
   
  1.5: 需要安装 autoconf    
  yum update    
  yum install -y  autoconf    
    
  安装autoconf成功后，继续make。报错：    
  make[1]: Entering directory `/root/ssdb-master/deps/leveldb-1.18'      
  g++ -I. -I./include -fno-builtin-memcmp -pthread -DOS_LINUX -DLEVELDB_PLATFORM_POSIX -DSNAPPY -I../snappy-1.1.0 -O2 -DNDEBUG -c             
  db/builder.cc -o db/builder.o     
  make[1]: g++: Command not found    
  make[1]: *** [db/builder.o] Error 127    
  make[1]: Leaving directory `/root/ssdb-master/deps/leveldb-1.18'    
  make: *** [all] Error 2    
    
  1.6：需要安装g++    
  yum -y install gcc+ gcc-c++    
  安装g++成功后，继续make。报错：（有的情况不报错。不报错代表可以跳过1.7  ）    
  g++: error: /root/ssdb-master/deps/snappy-1.1.0/.libs/libsnappy.a: No such file or directory    
  make[1]: *** [all] Error 1    
  make[1]: Leaving directory `/root/ssdb-master/src'    
  make: *** [all] Error 2     
    
  1.7： snappy 没有编译成功，因为你的系统时钟有问题，解决如下：（可参考官网介绍的这个错误的解决方案）    
  cd deps/snappy-1.1.0;    
  ./configure    
  make     
    
  1.8： 安装Snappy成功后，返回到ssdb-master目录     
  make     
  #默认将安装在 /usr/local/ssdb 目录下     
  make install    
    
  如果想安装在其他目录下：    
  sudo make install PREFIX=/your/direcotry    
    
  安装成功。    
  启动和停止等操作可参考官网说明。    
    
#### 2、centos安装SSDBAdmin(SSDB数据库的可视化界面管理工具)：github: https://github.com/jhao104/SSDBAdmin    
   2.1：提供SSDB数据的hash/zset/kv/queue等数据结构的增删改查等功能。需要依赖Python 3.x和Flask    
   2.2：先安装好Python 3.x，部分Linux默认自带Python 2，不要删除，直接安装Python 3， 使用Python3 命令来使用Python3.x    
   2.3：下载Python: wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz    
   2.4：然后新建一个目录：mkdir /usr/local/python3    
   2.5：Python解压到该目录：cd /usr/local/python3    
         tar zxvf /root/Python-3.6.2.tgz    
   2.6：进行配置、编译和安装：    
         cd Python-3.6.2    
         ./configure --prefix=/usr/local/python3    
         make    
         make install    
   2.7： 使用python3 -V 检查安装是否正确，如下图：   
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/1.png)   
  
   2.8： 安装Flask，可能会出现如下错误，可能是你机器访问不上 https://pypi.python.org/simple/click ，可以使用curl 测一下
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/2.png)    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/12.png)   
   
   2.9： 修改一下DNS配置，加上如图：nameserver 114.114.114.114  nameserver 8.8.8.8        
   vim /etc/resolv.conf     
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/11.png)     
   
   然后重新安装Flask：    
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/13.png)     
  
   2.9： 下载SSDBAdmin    
   
   https://github.com/jhao104/SSDBAdmin    
   
   2.10： 下载SSDBAdmin后解压，然后进入配置文件修改配置    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/3.png)    
   
   修改配置，将host和port修改成正确值    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/4.png)    
   
   2.11： 安装依赖包: pip3 install -r requirements.txt    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/14.png)    
   
   2.12： 启动: python3 run.py 
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/15.png)    
   
   让python服务在后台运行请使用：  nohup python3 run.py &     
   要使用exit退出shell    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/21.png)    
   
   2.13： 访问:http://127.0.0.1:5000/ssdbadmin    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/18.png)     
   
   hash结构的数据：    
   
   ![Image text](https://github.com/liweiDiao/ssdbDemo/blob/master/images/19.png)    
         

