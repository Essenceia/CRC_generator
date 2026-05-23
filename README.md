# CRC Generator for Verilog or VHDL

## Description 

CRC Generator is a command-line application that generates Verilog or VHDL code for CRC of any data width between 1 and 1024 and polynomial width between 1 and 1024. The code is written in C and is cross-platform compatible.

## Build 

To build on linux using gcc: 
```bash
g++ -o crc_gen crc_gen.cpp
```

## Usage 

Invoke the tool via command line:
```bash
./crc_gen [language] [data_width] [poly_width] [poly_string]
```
**Options:** 

- `language`: `verilog` or `vhdl` 
- `data_width`: data bus width ${1..1024}$
- `poly_width`: polynomial width ${1..1024}$
- `poly_string`: a string that describes CRC polynomial, eg: Ethernet MAC FCS uses poly `04C11DB7`

### Notes on `poly_string`

Examples: \
$05 = x^5+x^2+1$ \
$8005 = x^{16} + x^{15}+ x^2+ 1$

 
The string representation (`0x05`, `0x8005`) doesn’t include highest degree coefficient in polynomial representation ($x^5$ and $x^{16}$ in the above examples).

## Output Examples 

Printing usage:
```
./crc_gen

usage:
	crc-gen language data_width poly_width poly_string

parameters:
	language    : verilog or vhdl
	data_width  : data bus width {1..1024}
	poly_width  : polynomial width {1..1024}
	poly_string : polynomial string in hex

example: usb crc5 = x^5+x^2+1
	crc-gen verilog 8 5 05

```

Generation in action: 
```
./crc_gen verilog 2 4 3

//-----------------------------------------------------------------------------
// Copyright (C) 2009 OutputLogic.com
// This source file may be used and distributed without restriction
// provided that this copyright statement is not removed from the file
// and that any derivative work contains the original copyright notice
// and the associated disclaimer.
// THIS SOURCE FILE IS PROVIDED "AS IS" AND WITHOUT ANY EXPRESS
// OR IMPLIED WARRANTIES, INCLUDING, WITHOUT LIMITATION, THE IMPLIED	
// WARRANTIES OF MERCHANTIBILITY AND FITNESS FOR A PARTICULAR PURPOSE.
//-----------------------------------------------------------------------------
// CRC module for
//	 data[1:0]
//	 crc[3:0]=1+x^1+x^4;
//
module crc(
	input [1:0] data_in,
	input        crc_en,
	output [3:0] crc_out,
	input        rst,
	input        clk);

	reg [3:0] lfsr_q,
	           lfsr_c;
	assign crc_out = lfsr_q;
	always @(*) begin
		lfsr_c[0] = lfsr_q[2] ^ data_in[0];
		lfsr_c[1] = lfsr_q[2] ^ lfsr_q[3] ^ data_in[0] ^ data_in[1];
		lfsr_c[2] = lfsr_q[0] ^ lfsr_q[3] ^ data_in[1];
		lfsr_c[3] = lfsr_q[1];


	end // always

	always @(posedge clk, posedge rst) begin
		if(rst) begin
			lfsr_q  <= {4{1'b1}};
		end
		else begin
			lfsr_q  <= crc_en ? lfsr_c : lfsr_q;
		end
	end // always
endmodule // crc 
```

## About the original author 

Evgeni Stavinov is the creator and main developer of [OutputLogic.com](https://web.archive.org/web/20240718180127/http://outputlogic.com/). Evgeni has more than 20 years of diverse design experience in the areas of FPGA logic design, embedded software and communication protocols. He holds MSEE from University of Southern California and BSEE from Technion – Israel Institute of Technology. For more information contact evgeni@outputlogic.com

## License

This code is licensed under MIT license, all rights reserved to Evgeni Stavinov. See LICENSE for more details.
