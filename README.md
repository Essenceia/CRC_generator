# CRC Generator for Verilog or VHDL

## Description 

CRC Generator is a command-line application that generates Verilog or VHDL code for CRC of any data width between 1 and 1024 and polynomial width between 1 and 1024. The code is written in C and is cross-platform compatible.

## Usage 

Invoke the tool via command line:
```
./crc [language] [data_width] [poly_width] [poly_string]
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
C:\OutputLogic\lfsr-counter-generator> lfsr-counter-generator
  usage:
        lfsr-counter-generator language count
  parameters:
        language: verilog or vhdl
        count   : counter value in hex or decimal format, e.g. 1234, 0x1234
```
 
## About the original author 

Evgeni Stavinov is the creator and main developer of [OutputLogic.com](https://web.archive.org/web/20240718180127/http://outputlogic.com/). Evgeni has more than 20 years of diverse design experience in the areas of FPGA logic design, embedded software and communication protocols. He holds MSEE from University of Southern California and BSEE from Technion – Israel Institute of Technology. For more information contact evgeni@outputlogic.com

## License

This code is licensed under MIT license, all rights reserved to Evgeni Stavinov. See LICENSE for more details.
