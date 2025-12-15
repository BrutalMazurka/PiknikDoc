
# FS ( e

*Source: [*https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/fs_lparen_le.html*](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/fs_lparen_le.html)*

-----
## [Name]

Enable/disable Automatic Status Back (ASB) for optional functions (extended status)

## [Format]

|ASCII|FS|(|e|***pL***|***pH***|***m***|***n***|
| :- | :- | :- | :- | :- | :- | :- | :- |
|Hex|1C|28|65|***pL***|***pH***|***m***|***n***|
|Decimal|28|40|101|***pL***|***pH***|***m***|***n***|

## [Range]

(***pL*** + ***pH*** × 256) = 2 

***m*** = 51 

***n*** = 0 – 255 

## [Default]
***n*** = 0 

## [Description]
Enables or disables extended ASB (Automatic Status Back) and specifies the status items to include, using n as follows: 

|**n:** **Bits**|**Function**|**Binary**|**Hex**|**Decimal**|
| :- | :- | :- | :- | :- |
|0 – 2|(Reserved)|0|00|0|
|3|Command execution (offline) status disabled|0|00|0|
|3|Command execution (offline) status enabled|1|08|8|
|4 – 7|(Reserved)|0|00|0|

## [Notes]
- Bit 3 is available only when command execution (offline) is enabled.
- ASB (Automatic Status Back) transmits the status automatically to the printer in real-time. It is called [ASB function] and the status is [ASB status]. If you use the ASB, an application can acquire the printer change in real-time and passively. 
- Enabling any status (specifying ***n*** ≠ 0) starts extended ASB. Then the current extended ASB status is transmitted. After that, when ASB is active, the selected extended ASB status is transmitted each time the status changes. 
- When specifying ***n*** = 0, extended ASB is disabled. While ASB is disabled, the extended ASB status is not transmitted. 
- Multiple status items can be selected.
- When the ASB function is operating, even if the printer is specified as an invalid peripheral device with [ESC =](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_equal.html), the extended ASB status is transmitted if the status of the printer changes. 
- This command is effective until [ESC @](https://download4.epson.biz/sec_pubs/pos/reference_en/escpos/esc_atsign.html) is executed, the printer is reset, or the power is turned off. 
- All extended ASB status represents the enabled status whenever the status changes. Therefore, the disabled status items may change, because each status transmission represents the current status. 
- The extended ASB status is a 4-byte message as shown in the following table.

|**Send data**|**Hex**|**Decimal**|**Number of bytes**|
| :- | :- | :- | :- |
|Header|39h|57|1 byte|
|Status A (∗1)|See the Status A table below.|1 byte||
|Status B|40h|64|1 byte|
|NUL|00h|0|1 byte|

- (∗1) Status A is as follows:

  |**Bit**|**Function**|**Binary**|**Hex**|**Decimal**|
  | :- | :- | :- | :- | :- |
  |0|(Reserved)|1|01|1|
  |1|(Reserved)|0|00|0|
  |2|Receipt unit is online.|0|00|0|
  ||Receipt unit is offline.|1|04|4|
  |3|(Reserved)|0|00|0|
  |4|Command execution (offline) enabled|0|00|0|
  ||Command execution (offline) disabled|1|10|16|
  |5|(Reserved)|0|00|0|
  |6|Fixed|1|40|64|
  |7|Fixed|0|00|0|

- When block data [Header – NUL] is being transmitted, ASB status cannot be transmitted. Therefore, you cannot get the printer status change through the ASB status when Block data [Header – NUL] is transmitted. 
- The extended ASB status can be differentiated from other transmission data by the specific data of the transmission data block. When the printer transmits the header [Hex = 39h / Decimal = 57], data up to NUL [Hex = 00h / Decimal = 0] are processed as extended ASB status. 

