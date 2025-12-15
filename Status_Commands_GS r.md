
# GS r

*Source: [*https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lr.html*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lr.html)*

-----

## [Name]

Transmit status

## [Format]

|ASCII|GS|r|***n***|
| :- | :- | :- | :- |
|Hex|1D|72|***n***|
|Decimal|29|114|***n***|

## [Range]

***n***: different depending on the printers 

**TM-T20III:** 

***n*** = 1, 2, 49, 50 

## [Description]

Transmits the status using n as follows: 

|***n***|**Function**|
| :- | :- |
|1, 49|Transmits paper sensor status|
|2, 50|Transmits drawer kick-out connector status|
|4, 52|Transmits ink status|

## [Notes]
- Each status is 1 byte.
- Paper sensor status (***n*** = 1, 49) 

  |**Bit**|**Binary**|**Hex**|**Decimal**|**Status**|
  | :- | :- | :- | :- | :- |
  |0, 1|00|00|0|Roll paper near-end sensor: paper adequate.|
  |0, 1|11|03|3|Roll paper near-end sensor: paper not present.|
  |2, 3|00|00|0|Roll paper end sensor: paper present.|
  |2, 3|11|0C|12|Roll paper end sensor: paper not present.|
  |4|0|00|0|Fixed|
  |5, 6|−|−|−|(Reserved)|
  |7|0|00|0|Fixed|

- Some paper sensors are not present, depending on the printer model. The names of some paper sensors are different, depending on the printer model. 

- Drawer kick-out connector status (***n*** = 2, 50) 

  |**Bit**|**Binary**|**Hex**|**Decimal**|**Status**|
  | :- | :- | :- | :- | :- |
  |0|0|00|0|Drawer kick-out connector pin 3 is LOW. (∗1)|
  |0|1|01|1|Drawer kick-out connector pin 3 is HIGH. (∗1)|
  |1 – 3|−|−|−|(Reserved)|
  |4|0|00|0|Fixed|
  |5, 6|−|−|−|(Reserved)|
  |7|0|00|0|Fixed|

  (∗1) If the optional external buzzer is connected to the drawer kick-out connector, the bit is HIGH while the buzzer is sounding, and LOW otherwise. 

- Ink status (n = 4, 52) 

  |**Bit**|**Binary**|**Hex**|**Decimal**|**Function**|
  | :- | :- | :- | :- | :- |
  |0|0|00|0|Ink near-end not detected (1st color)|
  |0|1|01|1|Ink near-end detected (1st color)|
  |1|0|00|0|Ink near-end not detected (2nd color)|
  |1|1|02|2|Ink near-end detected (2nd color)|
  |2, 3|−|−|−|(Reserved)|
  |4|0|00|0|Fixed|
  |5, 6|−|−|−|(Reserved)|
  |7|0|00|0|Fixed|

- When you use this command, obey the following rules.
  - After the host PC transmits the function data, the printer will send response data or status data back to the PC. Do not transmit more data from the PC until the response data or status data are received from the printer. 
  - When operating with a serial interface, be sure to configure operation so that the host computer uses the printer only when it is READY. 
  - With a parallel interface, a real-time status is stored in the transmission buffer of the printer temporarily the same as the other transmission data (except for ASB status), and when the host enters reverse mode, data is transmitted in order from the beginning of the transmission buffer. The transmission buffer is 99 bytes; therefore, data that exceeds 99 bytes is ignored. When using this command, the host should be changed to the reverse mode immediately and execute a receive processing of status. 
- After the print changing line operation ends, paper sensor status (***n*** = 1, 49) is transmitted. Therefore if use **GS r 1** according to the printing instruction, host recognizes the print completion by receiving paper sensor status. 
- Normal status can be differentiated by the information of bits 4, and 7 from other transmission data. If the data transmitted from the printer after outputting [GS r](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lr.html#gs_lr) to the printer is "0xx1xx10" (x = 0 or 1), process the data as a normal status. 

**TM-T20III:** 

- Paper sensor status (***n*** = 1, 49) 
  - Bits 2 and 3: While the cover is open, this shows the state when the cover was still closed. (However, if command execution (offline) is disabled, this command is not processed while the cover is open.) 

