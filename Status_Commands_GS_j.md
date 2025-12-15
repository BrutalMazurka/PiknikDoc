
# GS j

*Source: [*https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lj.html*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/gs_lj.html)*

-----

## [Name]

Enable/disable Automatic Status Back (ASB) for ink

## [Format]

|ASCII|GS|j|***n***|
| :- | :- | :- | :- |
|Hex|1D|6A|***n***|
|Decimal|29|106|***n***|

## [Range]

***n*** = 0 – 255 

## [Default]

When DIP switch or memory switch (BUSY condition) is Off: ***n*** = 0 

When DIP switch or memory switch (BUSY condition) is On: ***n*** = 1 

## [Description]

Enables or disables the ink ASB (Automatic Status Back) and specifies the status items to include, using n as follows: 

|**n: Bit** |**Function**|**Binary**|**Hex**|**Decimal**|
| :- | :- | :- | :- | :- |
|0|Disable online/offline status of the ink mechanism|0|00|0|
||Enable online/offline status of the ink mechanism|1|01|1|
|1|Disable ink status detection|0|00|0|
||Enable ink status detection|1|02|2|
|2 – 7|(Reserved)|0|00|0|

## [Notes]
- ASB (Automatic Status Back) transmits the status such as ink near-end, ink cartridge installed/not installed automatically to the printer in real-time. It is called [ASB function] and the status is [ASB status]. If you use ASB, application can acquire the printer change in real-time and passively. 
- Enabling any status (except ***n*** = 0) starts ink ASB. Then the current ink ASB status is transmitted. After that, when ASB is active, the selected enabled ink ASB status is transmitted each time the status changes. 
- When ***n*** = 0, ink ASB is disabled. When ASB is disabled, ink ASB status is not transmitted. 
- If ASB is enabled when the printer is disabled by [ESC =](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_equal.html), the printer transmits a 4-byte status message whenever the status changes. 
- This command is effective until [ESC @](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_atsign.html) is executed, the printer is reset, or the power is turned off. 
- All ink ASB status items represent the enabled status whenever the status changes. Therefore, the disabled status items may change, because each status transmission represents the current status. 
- The ink ASB status, corresponding to each bit for ***n*** are as follows: 

|***n***|**ASB status description**|||
| :- | :- | :- | :- |
|**Bit**|**Status**|**ASB status**|**Bit**|
|0|Online/offline status of ink mechanism|Detect ink end|<p>Status A: Bit 1</p><p>Status B: Bit 1</p>|
|||Detect ink cartridge|<p>Status A: Bit 2</p><p>Status A: Bit 3</p>|
|||Cleaning|Status A: Bit 5|
|1|Ink detection status|Detect ink near-end|<p>Status A: Bit 0</p><p>Status B: Bit 0</p>|
|||Detect ink end|<p>Status A: Bit 1</p><p>Status B: Bit 1</p>|
|||Detect ink cartridge|<p>Status A: Bit 2</p><p>Status B: Bit 3</p>|

- The ink ASB status is a 4-byte message, consisting of the following table.

  |**Send data**|**Hex**|**Decimal**|**Number of bytes**|
  | :- | :- | :- | :- |
  |Header|35h|53|1 byte|
  |Status A (∗1)|40h – 7Fh|64 – 127|1 byte|
  |Status B (∗2)|40h – 7Fh|64 – 127|1 byte|
  |NUL|00h|0|1 byte|

- (∗1) Status A is shown in the table below:

  |**Bit**|**Function**|**Binary**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|Ink near-end not detected (1st color)|0|00|0|
  ||Ink near-end detected (1st color)|1|01|1|
  |1|Ink end not detected (1st color)|0|00|0|
  ||Ink end detected (1st color)|1|02|2|
  |2|Ink cartridge installed (1st color)|0|00|0|
  ||Ink cartridge not installed (1st color)|0|04|4|
  |3|Ink cartridge installed (2nd color)|0|00|0|
  ||Ink cartridge not installed (2nd color)|1|08|8|
  |4|(Reserved)|−|−|−|
  |5|Cleaning is not being performed|0|00|0|
  ||Cleaning is being performed|1|20|32|
  |6|Fixed|1|40|64|
  |7|Fixed|0|00|0|

- (∗2) Status B is shown in the table below:

  |**Bit**|**Function**|**Binary**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|Ink near-end not detected (2nd color)|0|00|0|
  ||Ink near-end detected (2nd color)|1|01|1|
  |1|Ink end not detected (2nd color)|0|00|0|
  ||Ink end detected (2nd color)|1|02|2|
  |2 – 5|(Reserved)|−|−|−|
  |6|Fixed|1|40|64|
  |7|Fixed|0|00|0|

- When block data [Header – NUL] is being transmitted, ASB status cannot be transmitted. Therefore, you cannot get the printer status change through ASB status when Block data [Header – NUL] is transmitted. 
- With a serial interface, the printer transmits a 4-byte ASB status message without confirming whether the host can receive data. 
- With a parallel interface, when ASB status is used, it is desirable for the host to be in a reverse idle state. However, if the host computer cannot always be in the reverse idle state, it is necessary to enter Reverse Mode regularly to watch for ASB status. If the host is not in the Reverse Mode for a long time, and the printer has to store ASB status changes to be transmitted, the following 2 sets (8 bytes) of ASB status are changed to special data and transmitted prior to other transmission data when the host enters Reverse Mode: 
  - ASB-1: Status information that shows whether status changes occurred
  - ASB-2: The latest ASB status information If bits have a different value between (ASB-1) and (ASB-2), this means at least one change has occurred. 

An example is shown below: 

|** |**Header**|**Status A**|**Status B**|**NUL**|
| :- | :- | :- | :- | :- |
|ASB-1|0011 0101|0110 0000|0100 0000|0000 0000|
|ASB-2|0011 0101|0100 0000|0100 0000|0000 0000|

Bit 5 of Status A for ASB-1 and ASB-2 is different. From this information, you can see that the printer executed a cleaning but it has already finished. 

- Ink ASB status can be differentiated from other transmission data by identified data of the transmission data group. If the header from the printer is [Hex = 35h/Decimal = 53], the host should process the data up to NUL [Hex = 00h/Decimal = 0] as ASB status.
