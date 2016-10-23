# Dol_Lab1
***
### Result
- example1.dot:

![example1](http://p1.bpimg.com/4851/e421c9c08ba4feaf.png)
- example2.dot:

![example2](http://p1.bpimg.com/4851/8cd0932c8f437763.png)

### Changes
- example1:例一是将Squre中的次方数从2次方改为3次方，所以首先分析squre.c文件
    ```
    #include <stdio.h>

#include "square.h"

void square_init(DOLProcess *p) {
    p->local->index = 0;
    p->local->len = LENGTH;
}

int square_fire(DOLProcess *p) {
    float i;

    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
    }

    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }

    return 0;
}

    ```
    我们可以发现这里其实就是在i=i*i处进行了平方，只要加上 *i 就可以变为3次方
- example2:例二是将原本的三个squre模块变为2个，我们来看example2.xml文件
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <processnetwork xmlns="http://www.tik.ee.ethz.ch/~shapes/schema/PROCESSNETWORK" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.tik.ee.ethz.ch/~shapes/schema/PROCESSNETWORK
    http://www.tik.ee.ethz.ch/~shapes/schema/processnetwork.xsd" name="example2"> 
    
      <variable value="2" name="N"/>
    
      <!-- instantiate resources -->
      <process name="generator">
        <port type="output" name="10"/>
        <source type="c" location="generator.c"/>
      </process>
    
      <iterator variable="i" range="N">
        <process name="square">
          <append function="i"/>
          <port type="input" name="0"/>
          <port type="output" name="1"/>
          <source type="c" location="square.c"/>
        </process>
      </iterator>
    
      <process name="consumer">
        <port type="input" name="100"/>
        <source type="c" location="consumer.c"/>
      </process>
    
      <iterator variable="i" range="N + 1">
        <sw_channel type="fifo" size="10" name="C2">
          <append function="i"/>
          <port type="input" name="0"/>
          <port type="output" name="1"/>
        </sw_channel>
      </iterator>
    
      <!-- instantiate connection -->
      <iterator variable="i" range="N">
        <connection name="to_square">
          <append function="i"/>
          <origin name="C2">
            <append function="i"/>
            <port name="1"/>
          </origin>
          <target name="square">
            <append function="i"/>
            <port name="0"/>
          </target>
        </connection>
    
        <connection name="from_square">
            <append function="i"/>
            <origin name="square">
              <append function="i"/>
              <port name="1"/>
            </origin>
            <target name="C2">
              <append function="i + 1"/>
              <port name="0"/>
            </target>
        </connection>
      </iterator>
    
      <connection name="g_">
        <origin name="generator">
         <port name="10"/>
        </origin>
        <target name="C2"> 
          <append function="0"/>
          <port name="0"/>
        </target>
      </connection>
    
      <connection name="_c">
        <origin name="C2">
          <append function="N"/>
          <port name="1"/>
        </origin>
        <target name="consumer">
          <port name="100"/>
        </target>
      </connection>
    
    </processnetwork>
    ```
    此时我们可以分析代码得出迭代的次数取决于代码开头定义的变量N，所以我们将N的值改为2即可。此时只会迭代两次，squre模块只创建了两次。
    
### Thinking
本次实验较为简单，没有什么难度，只要理解了DOL到底是什么，分析出每一个模块的作用很快就可以改好。主要还是理解DOL中各种模块。


