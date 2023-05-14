# Verilog_HDL review

Class: Verilog
Created: May 8, 2023 8:01 PM
Last edited by: 碧绿韭
Reviewed: Yes
Type: Lab

# Verilog HDL Review

Verilog语言是硬件描述语言，它用于数字电路设计。它允许设计人员描述和模拟数字系统，从而验证其正确性和功能。Verilog HDL是硬件描述语言的缩写，HDL代表硬件描述语言。Verilog代码可以通过编译器转换成实际的硬件电路，从而实现数字系统的设计。

Verilog语言的特点：

- Verilog具有高级语言的特点，使设计人员能够更容易地描述数字电路。
- Verilog语言具有层次结构，允许设计人员将电路分解成模块，使设计更易于管理。
- Verilog语言具有模块化设计，允许设计人员重复使用电路模块。
- Verilog语言允许设计人员描述时序行为，以确保电路的正确性和时序性。
- Verilog语言允许设计人员进行仿真和测试，以确保电路的正确性和功能。

# 实验二

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled.png)

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%201.png)

上图的下划线仅为容易读取位数只需要，并无实际含义。

注意output如果需要多次，则需要这样：output reg……：

在Verilog中，output和output reg的区别在于：

- output只能被赋值一次，而且只能被连接到一个wire或者一个reg。
- output reg可以被多次赋值，而且可以被连接到任何类型的wire或者reg。

如果需要在always块中对output进行赋值或者修改，那么需要使用output reg。

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%202.png)

## 译码选择7段数码管

**实验内容：**

- 使用“行为描述”设计一个“7段数码管译码选择”模块。
- 使用“结构描述”连接“译码显示模块”和“译码选择模块”，完成最终的实验要求（第一章）。

模块定义：

```verilog
module _7Seg_Driver_Choice(SW, SEG, AN, LED);
input [15:0] SW;   // 16位拨动开关
output [7:0] SEG;  // 7段数码管驱动，低电平有效
output [7:0] AN;// 7段数码管片选信号，低电平有效
output [15:0] LED;   // 16位LED显示
endmodule
```

**实验原理：**

- 用SW[3:0]输入待显示的单个数字。
- 输入数字与显示对应表如表3-2。
- 用SW[15:8]选择被驱动的7段数码管。
- 用LED[15:0]显示SW的状态。

图3-3译码选择7段数码管原理框图

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%203.png)

**知识点：**

- 学习使用“结构描述”设计一个模块。
- 学习调用模块。

### 实验二核心代码：

```verilog
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 2023/03/12 20:21:11
// Design Name: 
// Module Name: lab2
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////

module _7Seg_Driver_Choice(SW, SEG, AN, LED);
    input [15:0] SW;       // 16位拨动开关
    output reg [7:0] SEG;      // 7段数码管驱动，低电平有效
    output reg [7:0] AN;       // 7段数码管片选信号，低电平有效
    output [15:0] LED;     // 16位LED显示

     always@(SW[3:0]) 
            case(SW[3:0])
                4'b0000 : SEG[7:0] = 8'b11000000;
                4'b0001 : SEG[7:0] = 8'b11111001;
                4'b0010 : SEG[7:0] = 8'b10100100;
                4'b0011 : SEG[7:0] = 8'b10110000;
                4'b0100 : SEG[7:0] = 8'b10011001;
                4'b0101 : SEG[7:0] = 8'b10010010;
                4'b0110 : SEG[7:0] = 8'b10000010;
                4'b0111 : SEG[7:0] = 8'b11111000;
                4'b1000 : SEG[7:0] = 8'b10000000;
                4'b1001 : SEG[7:0] = 8'b10011000;
                4'b1010 : SEG[7:0] = 8'b10001000;
                4'b1011 : SEG[7:0] = 8'b10000011;
                4'b1100 : SEG[7:0] = 8'b11000110;
                4'b1101 : SEG[7:0] = 8'b10100001;
                4'b1110 : SEG[7:0] = 8'b10000110;
                default : SEG[7:0] = 8'b10001110;
            endcase
            
    always@(SW[15:13])  
        case(SW[15:13])  
            3'b000 : AN[7:0] = 8'b11111110;
            3'b001 : AN[7:0] = 8'b11111101;
            3'b010 : AN[7:0] = 8'b11111011;
            3'b011 : AN[7:0] = 8'b11110111;
            3'b100 : AN[7:0] = 8'b11101111;
            3'b101 : AN[7:0] = 8'b11011111;
            3'b110 : AN[7:0] = 8'b10111111;
            default : AN[7:0] = 8'b01111111;
        endcase
    assign LED[15:0] = SW[15:0];
endmodule
```

注意：在Verilog中，always @()是一个关键字，用于指定一个模块中的行为（behavioral）描述。在这种描述中，我们可以指定模块中的信号如何随时间变化，以及如何响应其他输入。更具体地说，always @()的作用是指定一个代码块在某个事件（如时钟上升沿）发生时执行。这使得我们可以模拟数字电路的行为，以便有效地测试和验证它们。

always @()语句的用法如下：

```verilog
always @(<sensitivity list>)
begin
    // Statements
end

```

"sensitivity list"列出了触发代码块执行的事件。如果事件发生，就会执行块中的语句。在事件列表中，我们可以列出信号名称，如时钟或输入端口，以指示当这些信号发生变化时应该执行行为。有几种不同的事件类型，包括：

- posedge：时钟上升沿
- negedge：时钟下降沿
- @(*)：任何信号的变化

在always @()块中，我们可以编写关于模块的行为的代码。这可以是任何有效的Verilog语句，包括赋值、条件语句、循环和函数调用等。

一个典型的例子是，我们可以在一个always @()代码块中编写一个状态机，通过检测信号的变化来移动到不同的状态。我们可以使用条件语句和赋值语句来指定在每个状态下应该执行的操作。通过这种方式，我们可以轻松地模拟数字电路的行为，并确保电路的正确性和时序性。

# 实验三：

### 注意wire的正确使用：

`wire` 是一种 Verilog 中的数据类型，用于表示单向信号线。`wire` 可以连接到其他信号线或寄存器的输入端口，但不能直接写入或更改其值。它通常用于表示从一个模块到另一个模块的信号传输。

`wire` 的定义格式：

```verilog
wire <width> <name>;
```

其中，`<width>` 表示信号的位宽，`<name>` 表示信号的名称。例如：

```
wire [7:0] data_out;
```

表示一个 8 位宽的输出信号 `data_out`。

`wire` 的作用是**连接模块的输入和输出**。在模块中，`wire` 被用作数据线，将输入端口连接到模块的输出端口，以实现信号传输。在模块之间，`wire` 可以用于连接两个模块的输出和输入。

`wire` 的用法示例：

```verilog
module example_module (
    input [7:0] data_in,
    output [7:0] data_out
);

  wire [7:0] data_internal;

  // some code that manipulates data_internal

  assign data_out = data_internal;

endmodule

```

在上面的代码中，`wire` 用于连接模块的输入和输出。输入信号 `data_in` 连接到模块内部的 `data_internal` 信号线，`data_internal` 信号线通过一些逻辑操作得到计算结果，最后连接到模块的输出信号 `data_out`。

总之，`wire` 通常用于表示一个单向连接，用于从一个模块到另一个模块的信号传输。它不能直接写入或更改其值，但可以连接到其他信号线或寄存器的输入端口。

在Verilog中，`initial`是一种关键字，用于指定初始值和行为。它用于在模拟器开始模拟之前执行一些初始化操作。它可以用于初始化寄存器、内存、变量等。

### 注意initial的正确使用：

`initial`的定义格式如下：

```
initial begin
  // Statements
end

```

其中，`begin` 和 `end` 用于指定要执行的语句块。在 `initial` 代码块中，我们可以编写任何有效的Verilog语句，包括赋值、条件语句、循环和函数调用等。在 `initial` 块中，语句按照书写顺序执行。

以下是一个 `initial` 块的示例。在这个例子中，我们使用 `initial` 块将一个寄存器的初始值设置为0：

```
module example_module (
    input clk,
    output reg [7:0] data_out
);

  reg [7:0] data_internal;

  initial begin
    data_internal = 8'b00000000;
  end

  always @(posedge clk) begin
    // some code that manipulates data_internal
    data_out <= data_internal;
  end

endmodule

```

在上面的代码中，我们定义了一个模块，它包含一个输入时钟信号 `clk` 和一个输出数据信号 `data_out`。在模块内部，我们定义了一个 8 位寄存器 `data_internal`。在 `initial` 中，我们将 `data_internal` 的初始值设置为0。在 `always` 块中，我们使用时钟信号 `clk` 对 `data_internal` 进行操作，并将其赋值给 `data_out` 输出信号。

总之，`initial` 用于指定在**模拟器开始模拟前执行的初始化操作**。它可以用于设置寄存器、内存、变量等的初始值，并且可以编写任何有效的Verilog语句。

### **Verilog中的赋值语句：**

1. 非阻塞赋值（Non-blocking Assignment）：用“<=”表示。非阻塞赋值是在时钟的上升沿（posedge）之后才执行，因此不会产生冲突。
2. 阻塞赋值（Blocking Assignment）：用“=”表示。阻塞赋值是立即执行的，它会占用模块的执行时间，因此会产生冲突。

**用法和用例：**

1. 非阻塞赋值：

```
always @(posedge clk)
begin
    a <= b;
end

```

在上述代码中，a的值将在时钟上升沿之后被赋值为b的值。

1. 阻塞赋值：

```
always @(posedge clk)
begin
    a = b;
end

```

在上述代码中，a的值将立即被赋值为b的值，这会占用模块的执行时间。

除了阻塞和非阻塞赋值，Verilog中还有其他类型的赋值语句，例如：

1. 连续赋值（Continuous Assignment）：用“assign”表示。连续赋值是用于将一个信号的值连续地赋给另一个信号，而不需要使用always块。它通常用于连接线和模块之间的信号。

```
assign b = a;

```

在上述代码中，b的值将连续地被赋值为a的值。

1. 传输赋值（Transport Delay Assignment）：用“#”表示。传输赋值是用于模拟数字电路中的传输延迟，它可以指定信号的延迟时间。

```
always @(posedge clk)
begin
    #1 a <= b;
end

```

在上述代码中，a的值将在时钟上升沿后1个时间单位被赋值为b的值，这表示a的值将在时钟上升沿后的下一个时间单位内更新。

总之，Verilog中有多种类型的赋值语句，包括非阻塞赋值、阻塞赋值、连续赋值和传输赋值。这些语句可以用于模拟数字电路中的行为，以及在模块之间传递信号。不同的赋值语句适用于不同的情况，需要根据具体情况选择合适的语句。

## 实验3.1

- nexys4ddr实验板的系统时钟信号由E3引脚提供，频率100MHz，周期10ns。设计一个频率为1Hz（周期为1s）的分频器divider，可在模块中用parameter定义一个参数delay，值为50,000,000，同时定义一个变量cnt对系统时钟clk的上升沿进行计数。每当cnt计数到delay（50,000,000×10ns=500,000,000ns=0.5s）时，将分频时钟信号CK的电平反转，即可得到周期为1s的分频时钟信号。在上层模块中实例化divider模块时，可传递新的参数来调整delay的值，得到合适频率的分频时钟信号。
- 设计上层模块lab3_1，实例化分频器模块divider，用分频器模块的输出信号控制led[0]闪亮。
- 在上层模块中修改分频器模块的参数delay，观察led[0]闪亮快慢的变化。
- 模块divider和lab3_1定义的框架已经在下面给出，请按以上要求完善代码，并在实验板上演示效果。

**3.1.2 实验步骤：**

- 创建工程，添加分频器模块，分频后的时钟控制一个LED闪亮。

分频是指将时钟信号的频率进行降低的时序电路，例如：如果要将频率降低为原来的1/N，就叫N分频。为便于高层模块改变分频系数N，将N定义为参数。实例化时可以把参数传递进去：

divider  ＃( 100000 ) D1（ ...）;

- 创建工程，添加分频器模块，分频后的时钟控制一个LED闪亮。

分频是指将时钟信号的频率进行降低的时序电路，例如：如果要将频率降低为原来的1/N，就叫N分频。为便于高层模块改变分频系数N，将N定义为参数。实例化时可以把参数传递进去：

- 添加约束文件： nexys4ddr实验板的系统时钟信号由E3引脚提供，频率为100MHz，将端口clk分配给E3，clk_N分配给LED0，使其由clk_N控制LED0闪烁。

### 补充：

Verilog中的模块实例化语句通常使用以下格式：

```
<module_name> <instance_name> (
  <port_name1> : <port_signal1>,
  <port_name2> : <port_signal2>,
  ...
);

```

其中，`<module_name>` 是要实例化的模块的名称，`<instance_name>` 是当前实例的名称。`<port_name>`是模块的端口名称，`<port_signal>`是当前实例连接到该端口的信号名称。多个端口可以用逗号隔开连接到一个模块实例上。在Verilog中，还可以使用参数指定实例中的某些属性。

模块实例化的示例：

```
module example_module (
  input [7:0] data_in,
  output [7:0] data_out
);

  // some code

endmodule

module top_module (
  input clk,
  output reg [7:0] led_out
);

  // instance of example_module
  example_module example_inst (
    .data_in(data_out),
    .data_out(data_in)
  );

  // some code

endmodule

```

在上面的代码中，我们定义了两个模块。`example_module` 包含一个输入信号 `data_in` 和一个输出信号 `data_out`。在 `top_module` 中，我们实例化了 `example_module`，并将 `data_out` 信号连接到 `data_in` 信号上，将 `data_in` 信号连接到 `data_out` 信号上。这种交叉连接的方式非常常见，通常用于在数字电路中交换信号。

在Verilog中，可以使用参数来指定模块实例的某些属性。参数可以在实例化模块时指定，也可以在模块定义中指定。参数可以是数字、字符串或其他数据类型。

使用参数的实例：

```verilog
module example_module (
  parameter WIDTH = 8,
  input [WIDTH-1:0] data_in,
  output [WIDTH-1:0] data_out
);

  // some code

endmodule

module top_module (
  input clk,
  output reg [7:0] led_out
);

  // instance of example_module with parameter
  example_module #(
    .WIDTH(16)
  ) example_inst (
    .data_in(data_out),
    .data_out(data_in)
  );

  // some code

endmodule

```

在上面的代码中，我们在 `example_module` 中定义了一个参数 `WIDTH`，用于指定数据信号的位宽。在 `top_module` 中，我们实例化了 `example_module`，并使用 `**#` 符号指定了 `WIDTH` 参数的值为16。这样，我们就可以在实例化模块时动态地改变参数值**，从而实现不同的功能。

### `parameter`

在Verilog语言中，`parameter`用于定义一个常量，并且可以在模块定义或实例化时指定其值。`parameter`的语法和变量定义类似，但是需要使用关键字`parameter`来声明，例如：

```
parameter WIDTH = 8;

```

`parameter`定义的常量可以在当前模块中的任何地方使用，而且它的值在编译时就已经确定，不能在运行时修改。`parameter`通常用于定义一些常量，例如数据位宽、时钟周期等，以便在整个模块中使用。

`parameter`的作用主要有以下几个方面：

1. 定义常量：`parameter`定义的常量是编译时确定的，不能在运行时修改，可以用于定义一些不变的常量，例如数据位宽、时钟周期等。
2. 代码重用：可以通过修改`parameter`的值来实现代码的重用，不同的参数值可以生成不同的硬件实现，从而实现不同的功能。
3. 代码优化：`parameter`可以用于调整硬件实现的性能和资源使用情况，例如通过调整数据位宽来减少存储空间的使用。

`parameter`的用法和用例：

`parameter`的用法和变量定义类似，定义一个常量时需要指定数据类型和初始值。例如：

```
parameter WIDTH = 8;

```

在模块定义或实例化时，可以通过指定参数的值来修改`parameter`的值，例如：

```
module example_module #(parameter WIDTH = 16) (
  input [WIDTH-1:0] data_in,
  output [WIDTH-1:0] data_out
);

  // some code

endmodule

module top_module (
  input clk,
  output reg [7:0] led_out
);

  // instance of example_module with parameter
  example_module #(
    .WIDTH(8)
  ) example_inst (
    .data_in(data_out),
    .data_out(data_in)
  );

  // some code

endmodule

```

在上面的代码中，我们使用`parameter`定义了一个常量`WIDTH`，并在模块定义和实例化时使用了该参数。在模块定义中，我们将`WIDTH`的默认值设为16，表示数据的位宽为16位。在实例化时，我们通过`#`符号指定`WIDTH`的值为8，从而实现了不同的数据位宽。

`parameter`的出现意义：

`parameter`是Verilog语言中非常重要的一个特性，它可以用于定义常量、重用代码、优化代码等。在硬件设计中，常常需要调整一些常量和参数，例如数据位宽、时钟周期等，这时可以使用`parameter`来定义这些常量，从而方便地进行调整和优化。

### 题目代码：

```verilog
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 2023/03/24 19:38:58
// Design Name: 
// Module Name: divide
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////

module divider(clk, clk_N);
    input clk;                      // 系统时钟
    output reg clk_N;                   // 分频后的时钟
    parameter N = 100_000_000;      // 1Hz的时钟,N=fclk/fclk_N
    reg [31:0] counter;             /* 计数器变量，通过计数实现分频。
                                       当计数器从0计数到(N/2-1)时，
                                       输出时钟翻转，计数器清零 */
    initial 
    begin
        counter =  0;
        clk_N = 0;
    end
 
    always @(posedge clk)  // 时钟上升沿
    begin    
        counter <= counter+1;
        if (counter > (N/2))
            begin  
            clk_N <= ~clk_N;
            counter <= 0 ;  
            end                                         
    end                      
endmodule
```

## 实验3.2

## 设计一个3位计数器

- 所谓计数器就是在时钟信号沿的控制下能够进行自动加或减（通常是+1或-1）的时序电路。

```verilog
module counter(clk, out);
    input clk;                    // 计数时钟
    output reg [2:0]  out;             // 计数值    
    
    always @(posedge clk)  begin  // 在时钟上升沿计数器加1
        out <= out+1;
    end                           
endmodule
```

## 实验3.4

### 设计只读存储器（ROM）

- 设计一个容量为8单元的4位只读存储器，里面存放待显示数字0、2、4、6、8、A、C、E 的4位二进制编码，可在initial语句中对存储器进行初始化。

`reg [3:0] mem [7:0];` 是一个Verilog代码，它声明了一个包含8个元素的4位寄存器数组。每个寄存器使用8位地址进行索引(`[7:0]`)，总共可以有8个存储单元。

```verilog
//	设计一个容量为8单元的4位只读存储器，里面存放待显示数字0、2、4、6、8、A、C、E 的4位二进制编码，可在initial语句中对存储器进行初始化。
module rom8x4(addr, data);
    input [2:0] addr;           // 地址
    output [3:0] data;          // 地址addr处存储的数据 
    reg [3: 0] mem [7: 0];      //  8个4位的存储器
    
    initial   // 初始化存储器
        begin             
            mem[0] = 4'b0000;
            mem[1] = 4'b0010;           
            mem[2] = 4'b0100;
            mem[3] = 4'b0110;
            mem[4] = 4'b1000;
            mem[5] = 4'b1010;
            mem[6] = 4'b1100;
            mem[7] = 4'b1110;         
        end     
    assign data[3:0] = mem[addr];                         // 读取addr单元的值输出                      
endmodule
```

## 实验3.7

设计使数码管动态显示的顶层模块，实现八个数码管从右至左“分时”显示存储器中的数据，即mem[0]值在AN0显示，mem[1]值在AN1显示，......

**module dynamic _scan(clk,  SEG, AN);**

**input clk;              // 系统时钟**

**output [7:0] SEG;              // 分别对应CA、CB、CC、CD、CE、CF、CG和DP**

**output [7:0] AN;        // 8位数码管片选信号**

**┋                      // 功能实现**

**endmodule**

- 可以通过按照下图所示连接各个模块，采用结构建模的方法来设实现dynamic _scan顶层模块。

```verilog
module dynamic_scan(clk,  SEG, AN);
    input clk;              // 系统时钟
    output [7:0] SEG;  		// 分别对应CA、CB、CC、CD、CE、CF、CG和DP
    output [7:0] AN;        // 8位数码管片选信号
    
    wire [2:0] count;
    wire [3:0] code;
    wire clk_N;
    divider(clk,clk_N);     
    counter(clk_N,count[2:0]);  
    rom8x4(count[2:0],code[3:0]);
    decoder_3to8(count[2:0] ,AN[7:0]);     
    decoder_4to16(code[3:0],SEG[7:0]);
           
endmodule
```

# 实验四

以下是一个简单的例子，用Verilog HDL实现一个简单的四位计数器：

```verilog
module counter(clk, count);
   input clk;
   output reg [3:0] count;
   always @(posedge clk) begin
       count <= count + 1;
   end
endmodule

```

以上代码定义了一个模块 `counter`，包含一个时钟输入 `clk` 和一个输出 `count`，其中 `count` 是一个四位的寄存器。在时钟上升沿的时候， `count` 的值加1。可以在另一个模块中实例化该计数器，并将其连接到其他模块。

```
module top_module(clk, out);
  input clk;
  output reg [3:0] count;
  counter c(clk, count);
  assign out = count;
endmodule
```

以上代码定义了一个顶层模块 `top_module`，包含一个时钟输入 `clk` 和一个输出 `out`，其中 `out` 是 `count` 的值。该模块实例化了计数器模块 `counter`，并将其连接到顶层模块中。当时钟上升沿的时候，计数器会计数并将值输出到 `out`。

在Verilog中，`assign`语句用于连续赋值，通常用于将某个信号连接到另一个信号或表达式上。与`always`块不同，`assign`语句不需要时钟信号，而且它们的赋值是在所有`always`块执行之前完成的。

`assign`语句的语法与`wire`声明类似，例如：

```
assign out = in1 & in2;

```

这将把输入信号 `in1` 和 `in2` 连接到逻辑与门中，并将结果赋值给输出信号 `out`。

在以下情况下，通常使用`assign`赋值：

- 连接信号：当需要将一个信号连接到另一个信号或表达式上时，可以使用`assign`语句来实现。例如，将一个输出端口连接到一个寄存器的输入端口。
- 简单逻辑：当需要执行简单的逻辑操作时，例如逻辑与、或、非或异或，可以使用`assign`语句来实现。这些操作不需要时序控制，因此不需要使用`always`块。
- 线网声明：当需要将一个信号声明为**线网**时，可以使用`assign`语句来实现。这通常在需要连接多个模块的信号时使用。

需要注意的是，`assign`语句不能用于分配寄存器或内存单元中的值。如果需要在时钟上升沿时存储数据，应该使用`always`块来实现。

在学习完实验3（简单时序电路设计）后，实验4将尝试较为复杂的时序电路设计（比如处理器）。此类电路设计主要包含“数据通路”（Datapath）和“控制器”（Controller）两大部分，在经典计算机模型中，处理器部分如图1红框所示。其中，数据通路负责数据的操作，包括算术运算和传输数据；控制器负责数据的控制，通常以有限状态机（FSM：Finite State Machine）方式实现，包括控制流的输入、输出，以及控制数据通路中数据的传输顺序。另外，处理器旁通常会有一个“存储器”（Memory），可根据地址存取程序指令和数据。注意，数据通路自身并不能工作，只能通过控制器输出控制信号，输入到数据通路的各个单元，才能完成处理器的工作。因此，一个经典处理器通常是由数据通路和控制器组合完成的；与之对应的，本实验共包含三个步骤：数据通路（步骤1），有限状态机（步骤2），和自动运算处理器（步骤3）。

## 数据通路设计

请参照实验样例，实现图3所示的数据通路。图3给出的数据通路里，SUM和NEXT是寄存器，Memory是存储器，+是加法器，==0是比较器，其它则是多路选择器。具体要求如下：

- 图中数据线的宽度和各个器件的数据线宽度初始设计时均为8位，要求构成数据通路时可以扩充至16位或者是32位；
- 设计的数据通路能够正确综合，Vivado所示的电路原理图与图3给出的一致。

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%204.png)

图3 数据通路图
【实验提示】
（1）分别设计n位加法器模块，n位2选1多路选择器模块，n位比较器模块。（用parameter传参来扩展）
（2）设计一个含同步复位rst和加载load端的n位寄存器模块
当load=1时，对输入的n位数据进行同步寄存，即让输入D的值赋给输出Q。
（3）设计一个n位存储器模块，存储器中存放如下的链表（具体见图7），链表第1个节点在0号地址，各节点的第一个地址存放下一个节点的地址，各节点的第二个地址中存放着要进行求和运算的数据，当下一个节点的地址为0时，表示到达链表的结尾，求和运算结束。
00000003
00000002
00000000
00000007
00000004
00000000
00000000
0000000b
00000006
00000000
00000000
00000000
00000008
00000000
00000000
00000000

注：存储器存放该链表的过程可以如下实现：1) 将该链表存入一个文本文件；

2) 用系统函数$readmemh读该文本文件对存储器进行初始化。具体可见教材readmemh的语法。

- 利用以上模块完成图3的数据通路模块的设计

输入端口有：时钟clk，复位rst，加载信号SUM_SEL, NEXT_SEL, A_SEL, LD_SUM, LD_NEXT。

输出端口有: 链尾标志NEXT_ZERO, 求和结果sum_out。

### 全加器

```verilog
module add(a,b,sum);
parameter WIDTH=8;
input [WIDTH-1:0]a,b;
output [WIDTH-1:0]sum;
assign sum =a+b;
endmodule
```

### 多路选择器

```verilog
module select1_2(a,b,sel,out);
parameter WIDTH=8;
input [WIDTH-1:0]a,b;
input sel;
output reg [WIDTH-1:0] out;
always @(*) begin
	case(sel)
	    2'b0:out=a;
	    2'b1:out=b;
	endcase
end
endmodule
```

### 寄存器

```verilog
module register(clk, rst, load, d, q);
  parameter WIDTH = 8;
  input clk, rst, load;
  input [WIDTH-1:0] d;
  output reg [WIDTH-1:0] q;
  always @(posedge clk) begin
    if (rst) q <=0;
    else if (load) q <= d;
  end    
endmodule
```

### 储存器

```verilog
module ram( read_addr, clk, q);
  parameter DATA_WIDTH = 8;
  parameter ADDR_WIDTH = 3;

  input clk;
  input [ADDR_WIDTH-1:0] read_addr;
  output reg [DATA_WIDTH-1:0] q;

  // 申明存储器数组
  reg [DATA_WIDTH-1:0] ram[2**ADDR_WIDTH-1:0];

initial begin                              //对存储器初始化
        $readmemh("E:\\verilog\\project_4\\project_4.srcs\\sources_1\\new\\ram_init.txt", ram);	end
   
  always @(*) begin
    q <= ram[read_addr];
end
endmodule
```

### 判断是否为0

```verilog
module iszero(d,q);
parameter WIDTH=8;
input [WIDTH-1:0] d;
output q;
assign q=(d==0)?1:0;
endmodule
```

### 数据通路

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%204.png)

```verilog
module datapath(clk, rst, SUM_SEL, NEXT_SEL, A_SEL, LD_SUM, LD_NEXT, NEXT_ZERO, sum_out);
input clk,rst,SUM_SEL,NEXT_SEL,A_SEL,LD_SUM,LD_NEXT;    
output NEXT_ZERO;
output wire [31:0] sum_out;
  wire [31:0] A,D, nextaddr, nextdata;
  wire [31:0] next,nextsum,S;
  
  select1_2 #(32) SEL_next(
  .a(0),
  .b(D),
  .sel(NEXT_SEL),
  .out(next));
  
  iszero #(32) ISZERO(
  .d(next),
  .q(NEXT_ZERO));
  
  register #(32) NEXT(
  .clk(clk), 
  .rst(rst),
  .load(LD_NEXT),
  .d(next),
  .q(nextaddr) );
 
	add #(32) ADD_A(
  .a(1),
  .b(nextaddr),
  .sum(nextdata));
  
  select1_2 #(32) SEL_A(
  .a(nextaddr),
  .b(nextdata),
  .sel(A_SEL),
  .out(A));
  
  ram #(32, 5) Memory (
  .read_addr(A),
  .clk(clk),
  .q(D));

	add #(32) ADD_SUM(
	.a(sum_out),
	.b(D),
	.sum(nextsum));

	select1_2 #(32) SEL_SUM(
	  .a(0),
	  .b(nextsum),
	  .sel(SUM_SEL),
	  .out(S));
	
	register #(32) SUM(
	  .clk(clk), 
	  .rst(rst),
	  .load(LD_SUM),
	  .d(S),
	  .q(sum_out) );

endmodule
```

## **有限状态机设计**

【实验样例】

给定某一类激光计时器（图4），不按按钮(即B=0)，激光器关闭(即X=0)；按了按钮(即B=1)，激光器会发射3个周期(即X=1)；3个周期后激光器关闭(即X=0)。

图4 激光计时器

该类激光计时器的有限状态机如图5所示，拥有Off（关闭），On1~On3（第1~3个周期激光发射）一共四个状态。每一个时钟周期都触发一次状态迁移。

【实验要求】

假设有限状态机的状态转移图如图6所示。根据状态转移图，按照有限状态机（FSM）标准的实现模式来编写Verilog程序代码。具体要求如下：

- 设计的有限状态机（FSM）能够正确综合；
- 编写有限状态机的**仿真程序**，完成有限状态机（FSM）的功能仿真，有限状态机功能仿真正确。

【实验提示】

该控制器模块的端口有：

输入端口：时钟clk，复位rst，启动求和start，链尾标志next_zero

输出端口:  控制信号LD_SUM,LD_NEXT,SUM_SEL,NEXT_SEL,A_SEL，求和结束DONE。

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%205.png)

**图6状态转移图**

**【实验填写】**

参照实验样例，根据实验提示完成实验要求，包括：

1. 图6的Verilog程序代码

module FSM(clk,rst,start,next_zero, LD_SUM, LD_NEXT, SUM_SEL, NEXT_SEL, A_SEL, DONE);

input clk,rst,start,next_zero;

output reg LD_SUM,LD_NEXT,SUM_SEL,NEXT_SEL,A_SEL,DONE;

endmodule

1. 设计testbench进行仿真测试，输入信号波形如下图：

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%206.png)

`timescale 1ns / 1ps

module FSM_tb();

endmodule

### 代码

1. 有限状态机

```verilog
module FSM(clk,rst,start,next_zero, LD_SUM, LD_NEXT, SUM_SEL, NEXT_SEL, A_SEL, DONE);
parameter S_START=0,S_COMPUTE_SUM=1,S_GET_NEXT=2,S_DONE=3;
input clk,rst,start,next_zero;
output reg LD_SUM,LD_NEXT,SUM_SEL,NEXT_SEL,A_SEL,DONE;
reg [1:0]STATE,STATENEXT;

always @(STATE,start,next_zero)begin
	case(STATE)
	S_START:begin
		LD_SUM<=0;
		LD_NEXT<=0;
		SUM_SEL<=0;
		NEXT_SEL<=0;
		A_SEL<=0;
		DONE<=0;
		if(start==0)
			STATENEXT<=S_START;
		else
			STATENEXT<=S_COMPUTE_SUM;
	end
	
	S_COMPUTE_SUM:begin
		LD_SUM<=1;
		LD_NEXT<=0;
		SUM_SEL<=1;
		NEXT_SEL<=1;
		A_SEL<=1;
		DONE<=0;
		STATENEXT<=S_GET_NEXT;
	end
	
	S_GET_NEXT:begin
		LD_SUM<=0;
		LD_NEXT<=1;
		SUM_SEL<=1;
		NEXT_SEL<=1;
		A_SEL<=0;
		DONE<=0;
		if(next_zero==0)
			STATENEXT<=S_COMPUTE_SUM;
		else
			STATENEXT<=S_DONE;
	end
	
	S_DONE:begin
		LD_SUM<=0;
		LD_NEXT<=0;
		SUM_SEL<=0;
		NEXT_SEL<=0;
		A_SEL<=0;
		DONE<=1;
		if(start==1)
			STATENEXT<=S_DONE;
		else
			STATENEXT<=S_START;
	end
	
	endcase
end

// StateReg 
always @(posedge clk) begin
  if (rst == 1 ) 
     STATE <= S_START;   //复位
  else
     STATE <= STATENEXT; //迁移到下一个状态
end

endmodule
```

## **自动运算电路的设计**

【实验要求】

将实验步骤1实现的数据通路与实验步骤2实现的有限状态机（FSM）结合起来，可以进行以链表方式存储的数据的求和运算。

在存储器中存放的数据链表（第5页所示链表）其结构如下图7所示，链表的各个节点在存储器中不是连续存放，各节点的第一个地址存放下一个节点的地址，各节点的第二个地址中存放着要进行求和运算的数据，当下一个节点的地址为0时，表示到达链表的结尾，求和运算结束。

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%207.png)

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%208.png)

**图7 数据链表及其在存储器中的存放格式**

利用上面设计的数据通路、有限状态机，将它们集成起来，设计并实现一个能够进行上述图7所示链表数据的自动求和运算，该电路的总体框架如图8所示。具体要求如下：

- 完成自动运算求和电路的设计，能够正确综合；
- 编写仿真程序，进行功能仿真，仿真结果正确；

![Untitled](Verilog_HDL%20review%2029a3027b9b2a4be584b2c4c0c0bae54d/Untitled%209.png)

**图8 自动运算电路模块构成图**

存储器初始化文件（存储器每个存储单元32位，共有16个存储单元，最后的求和运算结果 = 2+4+6+8 = 20）：

00000003

00000002

00000000

00000007

00000004

00000000

00000000

0000000b

00000006

00000000

00000000

00000000

00000008

00000000

00000000

00000000

【实验提示】

可参照实验2（简单组合电路设计）中的第四步“用2选1多路选择器构造3选1多路选择器。”利用结构描述，结合步骤1和步骤2的数据通路模块和有限状态机模块，构造自动运算电路，完成图7所示的数据链表的求和运算。

该控制器模块的端口有：

输入端口：时钟clk，复位rst，启动求和start

输出端口: 求和结束DONE，求和结果sum_out

### 代码

```verilog
module auto_add(clk,rst,start,DONE,sum_out);
input clk,rst,start;
output DONE;
output [31:0] sum_out;
wire next_zero,LD_SUM,LD_NEXT,SUM_SEL,NEXT_SEL,A_SEL;

FSM fsm(clk,rst,start,next_zero, LD_SUM, LD_NEXT, SUM_SEL, NEXT_SEL, A_SEL,DONE);
datapath dp(clk, rst, SUM_SEL, NEXT_SEL, A_SEL, LD_SUM, LD_NEXT, next_zero, sum_out);

endmodule
```

### 仿真代码

以下是Verilog实例化函数的示例：

```verilog
`timescale 1ns / 1ps

module auto_add_tb();
    reg clk,rst,start;
    wire DONE;
    wire [31:0] sum_out;
    auto_add a(clk,rst,start,DONE,sum_out);
    always #10 clk<=~clk;
    initial begin
        clk<=0;
        rst<=1;
        start<=0;
        @(posedge clk);
        #5 rst<=0;
        #5 start<=1;
    end
endmodule
```

```
module testbench();
  reg a, b;
  wire c;

  test_dut dut(
    .a(a),
    .b(b),
    .c(c)
  );

  initial begin
    a = 1'b0;
    b = 1'b1;
    #10;
    a = 1'b1;
    b = 1'b0;
    #10;
    a = 1'b0;
    b = 1'b0;
    #10;
    $finish;
  end
endmodule

module test_dut(
  input a,
  input b,
  output c
);
  xor(c, a, b);
endmodule

```

是的，实例化函数时传参应该使用`.原参数(参数)`的方式。这样可以确保代码的可读性和可维护性。