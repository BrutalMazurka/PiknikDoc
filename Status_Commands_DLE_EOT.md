
# DLE EOT

*Source: [*https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/dle_eot.html*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/dle_eot.html)*

-----

## [Name]
Transmit real-time status

## [Format]

|ASCII|DLE|EOT|***n***|***[a]***|
| :- | :- | :- | :- | :- |
|Hex|10|04|***n***|***[a]***|
|Decimal|16|4|***n***|***[a]***|

## [Range]
***n***, ***a***: different depending on the printers 

**TM-T20III:** 

***n*** = 1 − 4

## [Description]
Transmits the real-time status, using n as follows: 

|***n***|***a***|**Function**|
| :- | :- | :- |
|1|−|Transmit Printer status|
|2|−|Transmit Offline cause status|
|3|−|Transmit Error cause status|
|4|−|Transmit Roll paper sensor status|
|7|1|Transmit Ink status A|
||2|Transmit Ink status B|
|8|3|Transmit Peeler status|
|18|1|Transmit Interface status|
||2|Transmit DM-D status|

The parameter ***a*** is needed only when ***n*** = 7, 8 or 18. **DLE EOT BEL** is [DLE EOT](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/dle_eot.html#dle_eot) (***n*** = 7); for some previous printer models this command is called **DLE EOT BEL**. 
## [Notes]
- This is a Real-time command. Refer to [Notes of Real-time Commands](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/realtime_commands.html) for usage note. 
- When this command is transmitted, do not transmit data that follows until the corresponding status is received. 
- However, if this command must be transmitted continuously, it is possible to transmit up to 4 commands at once. 
- In this case, do not transmit data that follows until the all status is received.
- Be aware that if you designate continuous transmission in excess of the limit, the status may not be transmitted. 
- Each status consists of 1 byte, and the value is 0xx1xx10b.
- The real time status can be differentiated by the bits 0, 1, 4, and 7 from other transmission data, except for data in block data (Header – NUL).* 

- Printer status (***n*** = 1): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|Drawer kick-out connector pin 3 is LOW|00|0|
  ||1|Drawer kick-out connector pin 3 is HIGH|04|4|
  |3|0|Online|00|0|
  ||1|Offline|08|8|
  |4|1|Fixed|10|16|
  |5|0|Not waiting for online recovery|00|0|
  ||1|Waiting for online recovery|20|32|
  |6|0|Paper feed button is not being pressed|00|0|
  ||1|Paper feed button is being pressed|04|64|
  |7|0|Fixed|00|0|

- *Drawer kick-out connector pin 3 (bit 2) indicates the buzzer sounding status when the optional external buzzer is connected. It will be HIGH while sounding and LOW otherwise.* 
- *Online recovery wait (bit 5) is changed when [*GS ^*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_caret.html) is executed or the printer is waiting for the paper feed button to be pressed for removing a label or for roll paper to be replaced for some models.* 

- Offline cause status (***n*** = 2): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|Cover is closed|00|0|
  ||1|Cover is open|04|4|
  |3|0|Paper is not being fed by the paper feed button|00|0|
  ||1|Paper is being fed by the paper feed button|08|8|
  |4|1|Fixed|10|16|
  |5|0|No paper-end stop|00|0|
  ||1|Printing stops due to a paper-end|20|32|
  |6|0|No error|00|0|
  ||1|Error occurred|40|64|
  |7|0|Fixed|00|0|

- Error cause status (***n*** = 3): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|No recoverable error|00|0|
  ||1|Recoverable error occurred|04|4|
  |3|0|No autocutter error|00|0|
  ||1|Autocutter error occurred|08|8|
  |4|1|Fixed|10|16|
  |5|0|No unrecoverable error|00|0|
  ||1|Unrecoverable error occurred|20|32|
  |6|0|No auto-recoverable error|00|0|
  ||1|Auto-recoverable error occurred|40|64|
  |7|0|Fixed|00|0|

- *If recoverable error (bit 2) or autocutter error (bit 3) occurs due to paper jams or the like, it is possible to recover by correcting the cause of the error and executing [*DLE ENQ*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/dle_enq.html) (n = 2).* 
- *If an unrecoverable error (bit 5) occurs, turn off the power as soon as possible.*
- *The cause of the error can be checked by the offline response (when an offline cause is added). See Function 49 of [*GS ( H*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lparen_ch.html).* 

- Roll paper sensor status (***n*** = 4): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2, 3|00|Roll paper near-end sensor: paper adequate|00|0|
  ||11|Roll paper near-end sensor: paper near-end|0C|12|
  |4|1|Fixed|10|16|
  |5, 6|00|Roll paper end sensor: paper present|00|0|
  ||11|Roll paper end sensor: paper not present|60|96|
  |7|0|Fixed|00|0|

  *Some paper sensors are not present, depending on the printer model. The names of some paper sensors are different, depending on the printer model.* 

- Ink status A (***n*** = 7, ***a*** = 1): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|No ink near-end detected (1st color)|00|0|
  ||1|Ink near-end detected (1st color)|04|4|
  |3|0|No ink end detected (1st color)|00|0|
  ||1|Ink end detected (1st color)|08|8|
  |4|1|Fixed|10|16|
  |5|0|Ink cartridge detected (1st color)|00|0|
  ||1|Ink cartridge not detected (1st color)|20|32|
  |6|0|Cleaning is not being performed|00|0|
  ||1|Cleaning is being performed|40|64|
  |7|0|Fixed|00|0|

- Ink status B (***n*** = 7, ***a*** = 2): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|No ink near-end detected (2nd color)|00|0|
  ||1|Ink near-end detected (2nd color)|04|4|
  |3|0|No ink end detected (2nd color)|00|0|
  ||1|Ink end detected (2nd color)|08|8|
  |4|1|Fixed|10|16|
  |5|0|Ink cartridge detected (2nd color)|00|0|
  ||1|Ink cartridge not detected (2nd color)|20|32|
  |6|0|(Reserved)|00|0|
  |7|0|Fixed|00|0|



- Peeler status B (***n*** = 8, ***a*** = 3): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|Not waiting for a label to be removed|00|0|
  ||1|Waiting for a label to be removed|04|4|
  |3|0|(Reserved)|00|0|
  |4|1|Fixed|10|16|
  |5|0|Paper present in label peeling detector|00|0|
  ||1|No paper present in label peeling detector|20|32|
  |6|0|(Reserved)|00|0|
  |7|0|Fixed|00|0|

- Interface status (***n*** = 18, ***a*** = 1): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|Printing Using Multiple Interfaces disabled|00|0|
  ||1|Printing Using Multiple Interfaces enabled|04|4|
  |3|0|(Reserved)|00|0|
  |4|1|Fixed|10|16|
  |5, 6|0|(Reserved)|00|0|
  |7|0|Fixed|00|0|

- DM-D status (***n*** = 18, ***a*** = 2): 

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|1|Fixed|02|2|
  |2|0|DM-D transmission status is READY|00|0|
  ||1|DM-D transmission status is BUSY|04|4|
  |3|0|(Reserved)|00|0|
  |4|1|Fixed|10|16|
  |5, 6|0|(Reserved)|00|0|
  |7|0|Fixed|00|0|

**TM-T20III:** 

- Printer status (***n*** = 1) 
  - Bits 5 and 6 are not supported.
- Error cause status (***n*** = 3) 
  - Bit 2 is not supported.
- Roll paper sensor status (***n*** = 4) 
  - When the cover is open, the status of the roll paper end sensor (bit 5, 6) retain the value when the cover was closed immediately before. 

