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

## 1.2 test的结构        

一个test称为一个测试，一个典型的env内部结构如下图：  
![](.\pic\env0.PNG)   









