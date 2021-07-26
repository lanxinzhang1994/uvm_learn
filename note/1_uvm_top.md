# 1.典型UVM验证平台的结构    

## 1.1 验证平台的顶层     

一个典型的验证平台如下图：

**图1 - 典型uvm验证平台**

![](.\pic\tb1.PNG)

我们的验证环境顶层中包括test，dut，interface，clk_gen，rst_gen等几个部分，dut(desgin under test)就是我们的待测设计，而test是我们的验证组件，两者需要通过interface连接起来。同时dut和test的工作当然还少不了时钟和复位，因此还需要clk_gen和rst_gen提供时钟和复位信号。

对于仅熟悉verilog，却不太熟悉system verilog的工程师们来说，interface是一个陌生的概念。interface是一个system verilog引入的新的数据结构，在提高设计重用性和减少设计代码量的两个方面提供了不少便利。详情请参考system verilog的语法手册。  

如果设计比较复杂，比如dut有两个部分，一个部分进行数据运算，该部分对外留有一个数据交换的axi接口；同时还有一个特殊功能寄存器部分，用于控制dut行为与参数，该部分对外留有一个apb接口。那么验证平台可能就会略微复杂一点，如下图：   

**图2 - 略微复杂的uvm验证平台**    
![](.\pic\tb2.PNG)    

## 1.2 test和env的结构        

验证平台要对dut的很多feature进行测试，我们一般会在验证平台中设计很多test，每个test都验证了一个或者多个feature。

每个test又由若干env组成，env可以理解为，包含对某一个或者某些feature进行验证所需要的组件与数据事物的容器类。又或者说我们测试某一个或者某组feature时，需要针对feature的特征搭建包含激励的产生(sequencer)，发送(driver)，收集(monitor)，判断(scoreboard)等一系列组件，这些组件就组织在env这个容器类中。

关于env还有两点需要注意：

(1) env中是否可以例化子env？答案是可以，在验证平台比较复杂时，可以使用多级env的形式组织验证平台结构。

(2) 是否可以不使用env，而是直接将各个组件组织在test里？可以，但是不建议，这会导致test组织结构比较混乱，而且会降低可重用性。    

一个典型的env内部结构如下图：  
![](.\pic\env0.PNG)   

env中包含了一笔激励数据(transaction)流过的全部组件：sequencer产生transaction，driver将transaction驱动到接口上用于给dut施加激励，monitor从接口上获取dut的输出信号并转化为transaction，scoreboard将收到的transcation与参考值比较进行正确性的判断。通常我们会将sequencer，driver，monitor放置于一个称为agent的容器中。之所以将sequencer，driver，monitor放置于一个agent容器中，是因为driver，monitor以及sequencer经常用于组成测试平台同一个通信协议的数据交互。比如我们会把axi的driver，monitor以及sequencer封装在axi_agent中，而apb的driver，monitor以及sequencer封装在apb_agent中。axi_agent和apb_agent分别在axi_env和apb_env中实例化。









