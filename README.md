# Implementação de um processador em verilog em uma FPGA

## O que é um processador e como eles funcionam?

O processador é a unidade central de processamento de um computador (CPU), que funciona como o cérebro do computador, pois interage e faz as conexões necessárias entre todos os programas instalados.

## O que é uma FPGA?

FPGA ou field-programmable gate array (ou ainda matriz de portas programáveis) é um dispositivo lógico programável que suporta a implementação de circuitos digitais.

## O  que é verilog?

Verilog é uma linguagem de descrição de hardware.

## Exemplo Contador implementado em verilog na FPGA


https://user-images.githubusercontent.com/93041836/208499291-58de9a90-d827-4e0b-9ade-c7d1589ae9ec.mp4


## A implementação:

A implementação é feita da seguinte maneira...

![KitFPGAde10Lite](https://user-images.githubusercontent.com/93041836/208439524-34f10994-472f-48f5-97b7-bb9bf559d4c3.png)

### ULA

Unidade lógica aritmética (ULA) é ...
```
module ALU
#(
parameter WIDTH = 4)
(
input [WIDTH-1:0] SrcA,
input [WIDTH-1:0] SrcB,
input [2:0] ALUControl,
output reg [WIDTH-1:0] ALUResult,
output reg Zero
);
// your code goes here…


    always @(*)
    begin
        case(ALUControl)
          3'b000: // Addition
           ALUResult = SrcA + SrcB ; 
	  3'b001: // Subtraction
           ALUResult = SrcA - SrcB ;
          3'b010: //  Logical and 
           ALUResult = SrcA & SrcB;
          3'b011: //  Logical or
           ALUResult = SrcA | SrcB;
          3'b101: // Greater comparison SLT
           ALUResult = (SrcA<SrcB)? 1 : 0 ;
          default: ALUResult = 0; 
        endcase
		  Zero = (ALUResult == 0)? 1 : 0 ;
    end


endmodule
```

```
module DE10_LITE_ALU(

	//////////// CLOCK //////////
	input 		          		ADC_CLK_10,
	input 		          		MAX10_CLK1_50,
	input 		          		MAX10_CLK2_50,

	//////////// KEY //////////
	input 		     [1:0]		KEY,

	//////////// LED //////////
	output		     [9:0]		LEDR,

	//////////// SW //////////
	input 		     [9:0]		SW
);

//=======================================================
//  REG/WIRE declarations
//=======================================================

//=======================================================
//  Structural coding
//=======================================================
// Instantiate ALU design and connect with Testbench variables

ALU #(
.WIDTH (4))
ALU0 ( .SrcA (SW[3:0]),
.SrcB (SW[7:4]),
.ALUControl ({~KEY[0],SW[9:8]}),
.ALUResult (LEDR[3:0]),
.Zero(LEDR[4]));

endmodule
```
### Registradores

Registradores são ....
```
module DE10_LITE_RegBank(

	//////////// CLOCK //////////
	input 		          		ADC_CLK_10,
	input 		          		MAX10_CLK1_50,
	input 		          		MAX10_CLK2_50,

	//////////// KEY //////////
	input 		     [1:0]		KEY,

	//////////// LED //////////
	output		     [9:0]		LEDR,

	//////////// SW //////////
	input 		     [9:0]		SW
);

//=======================================================
//  REG/WIRE declarations
//=======================================================

//=======================================================
//  Structural coding
//=======================================================

regbank #(
.WIDTH (4),.SIZE(2))
RB0 ( .CLK (KEY[0]),
.WE3 (~KEY[1]),
.A1 (SW[1:0]),
.A2 (SW[3:2]),
.A3 (SW[5:4]),
.WD3 (SW[9:6]),
.RD1 (LEDR[3:0]),
.RD2 (LEDR[7:4]));

endmodule
```

### Máquina de estados finitos

Uma máquina de estados finita (FSM - do inglês Finite State Machine) ou autômato finito é um modelo matemático usado para representar programas de computadores ou circuitos lógicos. O conceito é concebido como uma máquina abstrata que deve estar em um de um número finito de estados. A máquina está em apenas um estado por vez, este estado é chamado de estado atual. Um estado armazena informações sobre o passado, isto é, ele reflete as mudanças desde a entrada num estado, no início do sistema, até o momento presente. Uma transição indica uma mudança de estado e é descrita por uma condição que precisa ser realizada para que a transição ocorra. Uma ação é a descrição de uma atividade que deve ser realizada num determinado momento.

## Referências:

https://tecnoblog.net/responde/o-que-e-um-processador/

https://embarcados.com.br/fpga/

https://pt.wikipedia.org/wiki/M%C3%A1quina_de_estados_finita

https://github.com/CarlosCraveiro/RISCV_based_processor

Aulas de Organização e Arquitetura de Computadores ministradas pelo Professor Simões

Aulas de Laboratório de Sistemas Digitais ministradas pelo Professor Maximillian Luppe
