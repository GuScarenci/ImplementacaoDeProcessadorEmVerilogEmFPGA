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

## Referências:

https://tecnoblog.net/responde/o-que-e-um-processador/

https://embarcados.com.br/fpga/
