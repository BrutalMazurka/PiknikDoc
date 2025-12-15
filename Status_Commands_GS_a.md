
# GS a

*Source: [*https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_la.html*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_la.html)*

-----

## [Name]
Enable/disable Automatic Status Back (ASB)

## [Format]

|ASCII|GS|a|***n***|
| :- | :- | :- | :- |
|Hex|1D|61|***n***|
|Decimal|29|97|***n***|

## [Range]

***n*** = 0 – 255 

## [Default]

***n***: different depending on the printers 

**TM-T20III:** 

***n*** = 0: When memory switch [Msw 1-3] is OFF 

***n*** = 2: When memory switch [Msw 1-3] is ON 

## [Description]

Enables or disables basic ASB (Automatic Status Back) and specifies the status items to include, using n as follows: 

|***n*: Bit** |**Binary**|**Function**|**Hex**|**Decimal**|
| :- | :- | :- | :- | :- |
|0|0|Drawer or optional external buzzer status disabled.|00|0|
||1|Drawer or optional external buzzer status enabled.|01|1|
|1|0|Online/offline status disabled.|00|0|
||1|Online/offline status enabled.|02|2|
|2|0|Error status disabled.|00|0|
||1|Error status enabled.|04|4|
|3|0|Roll paper sensor status disabled.|00|0|
||1|Roll paper sensor status enabled.|08|8|
|4,5|0|(Reserved)|00|0|
|6|0|Panel switch status disabled.|00|0|
||1|Panel switch status enabled.|40|64|
|7|0|(Reserved)|00|0|

## [Notes]
- ASB is the function that transmit the status of [cover open/close], [Online/Offline] from the printer automatically. It is called [ASB function] and the status is [ASB status]. If you use ASB, application can acquire the printer change in a real-time and passively. 
- Select any status enabled (except ***n*** = 0) and basic ASB starts. Then transmit the current basic ASB status. After that, while ASB is active the selected enabled basic ASB status is transmitted whenever the status changes. 
- When ***n*** = 0, basic ASB is disabled. When ASB is disabled, basic ASB status is not transmitted. 
- Multiple status items can be selected.
- When ASB is active, ASB status is transmitted whenever the status changes even if the printer is disabled by [ESC =](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_equal.html). 
- This command setting is effective until [ESC @](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_atsign.html) is executed, the printer is reset or power is turned off. 
- Any basic ASB status represents the enabled status whenever the status changes. Therefore the disabled status items may change, because each status transmission represents the current status. 

- The basic ASB statuses, corresponding to each bit for n are as follows: 

  |***n***|**ASB status**|||
  | :- | :- | :- | :- |
  |**Bit**|**Function**|**Bit**|**Status**|
  |0|Drawer kick-out connector status|Bit 2 of the first byte|Drawer kick-out connector pin 3 status|
  |1|Online/offline status|Bit 3 of the first byte|Online/ offline status|
  |||Bit 5 of the first byte|Cover status|
  |||Bit 6 of the first byte|Paper is being fed by paper feed button status|
  |||Bit 0 of the second byte|Waiting for online recovery status|
  |||Bit 0 and 1 of the third byte (∗1)|Roll paper near-end sensor status|
  |||Bit 2 and 3 of the third byte (∗1)|Roll paper end sensor status|
  |2|Error status|Bit 2 of the second byte|Recoverable error status|
  |||Bit 3 of the second byte|Autocutter error status|
  |||Bit 5 of the second byte|Unrecoverable error status|
  |||Bit 6 of the second byte|Automatically recoverable error status|
  |3|Roll paper sensor status|Bits 0 and 1 of the third byte|Roll paper near-end sensor status|
  |||Bits 2 and 3 of the third byte|Roll paper end sensor status|
  |6|Panel switch status|Bit 1 of the second byte|Paper feed status|

  (∗1) The bits are valid in case the sensor is selected to stop printing with [ESC c 4](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_lc_4.html). 

- Basic ASB status is 4-byte configuration [first byte – fourth byte].

- First byte (printer information):

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Fixed|00|0|
  |1|0|Fixed|00|0|
  |2|0|Drawer kick-out connector pin 3 is LOW. (∗1)|00|0|
  ||1|Drawer kick-out connector pin 3 is HIGH. (∗1)|04|4|
  |3|0|Online.|00|0|
  ||1|Offline.|08|8|
  |4|1|Fixed|10|16|
  |5|0|Cover is closed.|00|0|
  ||1|Cover is open.|20|32|
  |6|0|Paper is not being fed by the paper feed button.|00|0|
  ||1|Paper is being fed by the paper feed button.|40|64|
  |7|0|Fixed|00|0|

  (∗1) If the optional external buzzer is connected to the drawer kick-out connector, the bit is HIGH while the buzzer is sounding, and LOW otherwise. 

- Second byte (printer information):

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|0|Not waiting for online recovery.|00|0|
  ||1|Waiting for online recovery.|01|1|
  |1|0|Paper feed button is not pushed (off)|00|0|
  ||1|Paper feed button is pushed (on)|02|2|
  |2|0|No recoverable error (except for autocutter error).|00|0|
  ||1|Recoverable error occurred (except for autocutter error).|04|4|
  |3|0|No autocutter error.|00|0|
  ||1|Autocutter error occurred.|08|8|
  |4|0|Fixed|00|0|
  |5|0|No unrecoverable error.|00|0|
  ||1|Unrecoverable error occurred.|20|32|
  |6|0|No automatically recoverable error.|00|0|
  ||1|Automatically recoverable error occurred.|40|64|
  |7|0|Fixed|00|0|

- Online recovery wait (bit 0) is changed when [GS ^](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_caret.html) is executed, the printer waits for the button to be pressed for removing a label, or roll paper to be replaced for some models. 
- If recoverable error (bit 2) or autocutter error (bit 3) occurs due to paper jams or the like, it is possible to recover by correcting the cause of the error and executing [DLE ENQ](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/dle_enq.html) (n = 2). 
- If an unrecoverable error (bit 5) occurs, turn off the power as soon as possible.
- The cause of the error can be checked by the offline response (when an offline cause is added). See [GS ( H   <Function 49> ](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lparen_ch_fn49.html). 

- Third byte (paper sensor information):

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0, 1|00|Roll paper near-end sensor: paper adequate.|00|0|
  ||11|Roll paper near-end sensor: paper near-end.|03|3|
  |2, 3|00|Roll paper end sensor: paper present.|00|0|
  ||11|Roll paper end sensor: paper not present.|0C|12|
  |4|0|Fixed|00|0|
  |5, 6|−|(Reserved)|−|−|
  |7|0|Fixed|00|0|

- Some paper sensors are not present, depending on the printer model. The names of some paper sensors are different, depending on the printer model. 

- Fourth byte (paper sensor information):

  |**Bit**|**Binary**|**Status**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0 – 3|−|(Reserved)|−|−|
  |4|0|Fixed|00|0|
  |5, 6|−|(Reserved)|−|−|
  |7|0|Fixed|00|0|

- During Block data [header – NUL] transmission, ASB is disabled temporarily. Therefore you cannot get the printer status change through ASB status when block data [header – NUL] is transmitted. 
- With a serial interface, the printer transmits a 4-byte ASB status message without confirming whether the host can receive data. 
- With a parallel interface, when ASB status is used, it is desirable for the host to be in a reverse idle state. However, if the host computer cannot always be in the reverse idle state, it is necessary to enter Reverse Mode regularly to watch for ASB status. If the host is not in the Reverse Mode for a long time, and the printer has to store ASB status changes to be transmitted, the following 2 sets (8 bytes) of ASB status are changed to special data and transmitted prior to other transmission data when the host enters Reverse Mode: 
  - ASB-1: Status information that shows whether status changes occurred
  - ASB-2: The latest ASB status information

If bits have a different value between (ASB-1) and (ASB-2), this means at least one change has occurred. An example is shown below: 

|** |**First byte**|**Second byte**|**Third byte**|**Fourth byte**|
| :- | :- | :- | :- | :- |
|ASB-1|0011 1000|0000 0000|0110 0011|0000 1111|
|ASB-2|0001 0000|0000 0000|0110 0011|0000 1111|

Bit 5 and 3 of the first byte are different from (ASB-1) and (ASB-2). From this information, you can see that [The cover is shutting now and On line though Off line (Bit 3) by cover opening Bit 5)]. 

- Basic ASB status can be differentiated by other transmission data by Bit 0, 1, 4, and 7 of the first byte. Process the transmitted data from the printer as ASB status which is consecutive 3 byte if it is "0xx1xx00" [x = 0 or 1]. However, the processing shown in the following is necessary in the identifying processing of ASB status. 
  - When the host communicates with the printer by XON/XOFF control, 4 bytes of data may interrupt ASB status; therefore, 4-byte code except for the XOFF code, is processed as ASB status. ASB status configuration is different from that of the XOFF code. 

**TM-T20III:** 

The default value is set by Msw1-3.

- Second byte (printer information)
  - Bits 0, 1, and 2 are undefined.
- Third byte (paper sensor information)
  - When the cover is open, the status of the roll paper end sensor (bit 2, 3) retains the value when the cover was closed immediately before. 

