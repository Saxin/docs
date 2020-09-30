
GPIO functions define the action being taken by a device connected to it's pin. The current ESP8266 GPIO function configuration supports 216 GPIO functions like Relay1 (21) or Inverted Button1 (122).

The current implementation supports up to 250 functions as it is confined to 8-bits uint8_t arrays.

As more devices need to be connected it would become impossible to have them addressed with this 8-bit scheme. Another problem is that the current implementation would need GUI drop down boxes with up to 250 items which is hard to use. Also user templates still need to be able to address all possible GPIO functions.

To solve this issue I needed to change from 8-bit uint8_t to 16-bit uint16_t arrays. Without further action this would result in even more entries in the GUI drop down boxes and made adding more relays/buttons/switches etc also very cumbersome. As the ESP32 would need to be supported too a challenge lay ahead.

I decided for the following option where every GPIO function type (relays, buttons, switches) would have an id allowing for 32 devices. So every GPIO function would be offset by 32 (or 5-bits). These 5-bits can be used in the future to easily expand current max values like 4 buttons or 8 relays to max 32. How this works could have been seen in the ESP32 implementation as released some month ago. The GUI now contains two drop down boxes, one for the GPIO function (Relay) and one for the function index (1 to 8).

For the ESP8266 the current maxed GPIO functions of 216 will be fixed an any new device will be using the GPIO addressing scheme. To make transistion transparent a table is used to convert current 216 GPIO 8-bit function numbers to new 16-bit function numbers. As both are offset from 0 there will be some duplication in numbering so the only way to distinguish them by external programs like TDM is by checking the Tasmota verion number; Below 9.0 will be 8-bit GPIO, Starting with development version 9.0.0.1 it will be 16-bit GPIO.

See below how the numbers are being converted. The first number is 8-bit GPIO, the second the 16-bit GPIO.

## GPIO Conversion

|Old GPIO|New GPIO|
|-----|------|
| 0   | 0    |
| 1   | 1184 |
| 2   | 1216 |
| 3   | 1248 |
| 4   | 1312 |
| 5   | 608  |
| 6   | 640  |
| 7   | 1376 |
| 8   | 1056 |
| 9   | 160  |
| 10  | 161  |
| 11  | 162  |
| 12  | 163  |
| 13  | 164  |
| 14  | 165  |
| 15  | 166  |
| 16  | 167  |
| 17  | 32   |
| 18  | 33   |
| 19  | 34   |
| 20  | 35   |
| 21  | 224  |
| 22  | 225  |
| 23  | 226  |
| 24  | 227  |
| 25  | 228  |
| 26  | 229  |
| 27  | 230  |
| 28  | 231  |
| 29  | 256  |
| 30  | 257  |
| 31  | 258  |
| 32  | 259  |
| 33  | 260  |
| 34  | 261  |
| 35  | 262  |
| 36  | 263  |
| 37  | 416  |
| 38  | 417  |
| 39  | 418  |
| 40  | 419  |
| 41  | 420  |
| 42  | 352  |
| 43  | 353  |
| 44  | 354  |
| 45  | 355  |
| 46  | 448  |
| 47  | 449  |
| 48  | 450  |
| 49  | 451  |
| 50  | 452  |
| 51  | 1088 |
| 52  | 288  |
| 53  | 289  |
| 54  | 290  |
| 55  | 291  |
| 56  | 320  |
| 57  | 321  |
| 58  | 322  |
| 59  | 323  |
| 60  | 1408 |
| 61  | 1440 |
| 62  | 1472 |
| 63  | 1504 |
| 64  | 1600 |
| 65  | 1632 |
| 66  | 768  |
| 67  | 800  |
| 68  | 992  |
| 69  | 1696 |
| 70  | 1760 |
| 71  | 1792 |
| 72  | 1824 |
| 73  | 1856 |
| 74  | 1888 |
| 75  | 1920 |
| 76  | 1952 |
| 77  | 1984 |
| 78  | 2016 |
| 79  | 2048 |
| 80  | 2080 |
| 81  | 2112 |
| 82  | 192  |
| 83  | 193  |
| 84  | 194  |
| 85  | 195  |
| 86  | 196  |
| 87  | 197  |
| 88  | 198  |
| 89  | 199  |
| 90  | 64   |
| 91  | 65   |
| 92  | 66   |
| 93  | 67   |
| 94  | 384  |
| 95  | 385  |
| 96  | 386  |
| 97  | 387  |
| 98  | 1536 |
| 99  | 1568 |
| 100 | 2144 |
| 101 | 1728 |
| 102 | 2176 |
| 103 | 2208 |
| 104 | 2240 |
| 105 | 1120 |
| 106 | 1152 |
| 107 | 2272 |
| 108 | 2304 |
| 109 | 2336 |
| 110 | 2368 |
| 111 | 832  |
| 112 | 864  |
| 113 | 896  |
| 114 | 928  |
| 115 | 960  |
| 116 | 2400 |
| 117 | 2432 |
| 118 | 2464 |
| 119 | 2496 |
| 120 | 2528 |
| 121 | 2560 |
| 122 | 96   |
| 123 | 97   |
| 124 | 98   |
| 125 | 99   |
| 126 | 128  |
| 127 | 129  |
| 128 | 130  |
| 129 | 131  |
| 130 | 2592 |
| 131 | 2624 |
| 132 | 2656 |
| 133 | 2688 |
| 134 | 2720 |
| 135 | 2752 |
| 136 | 2784 |
| 137 | 2816 |
| 138 | 2848 |
| 139 | 2880 |
| 140 | 2912 |
| 141 | 2944 |
| 142 | 2976 |
| 143 | 3008 |
| 144 | 3040 |
| 145 | 3072 |
| 146 | 3104 |
| 147 | 3136 |
| 148 | 3200 |
| 149 | 3232 |
| 150 | 3264 |
| 151 | 3296 |
| 152 | 3265 |
| 153 | 3297 |
| 154 | 3392 |
| 155 | 3424 |
| 156 | 3456 |
| 157 | 544  |
| 158 | 576  |
| 159 | 3168 |
| 160 | 480  |
| 161 | 512  |
| 162 | 1024 |
| 163 | 3488 |
| 164 | 3520 |
| 165 | 3552 |
| 166 | 3584 |
| 167 | 3616 |
| 168 | 3648 |
| 169 | 3680 |
| 170 | 3712 |
| 171 | 3744 |
| 172 | 3776 |
| 173 | 3808 |
| 174 | 3840 |
| 175 | 3872 |
| 176 | 3904 |
| 177 | 3936 |
| 178 | 3968 |
| 179 | 4000 |
| 180 | 4032 |
| 181 | 4064 |
| 182 | 4096 |
| 183 | 4128 |
| 184 | 4160 |
| 185 | 4192 |
| 186 | 4224 |
| 187 | 4256 |
| 188 | 4288 |
| 189 | 4320 |
| 190 | 4352 |
| 191 | 4384 |
| 192 | 1344 |
| 193 | 1280 |
| 194 | 4416 |
| 195 | 4448 |
| 196 | 4480 |
| 197 | 4512 |
| 198 | 4544 |
| 199 | 4576 |
| 200 | 4608 |
| 201 | 4640 |
| 202 | 4672 |
| 203 | 1664 |
| 204 | 4928 |
| 205 | 4960 |
| 206 | 4992 |
| 207 | 5056 |
| 208 | 5088 |
| 209 | 5120 |
| 210 | 5152 |
| 211 | 5184 |
| 212 | 5216 |
| 213 | 5248 |
| 214 | 5280 |
| 215 | 5312 |
| 216 | 5344 |

## ADC Conversion

| Old ADC | New ADC    |
|-----|------|
| 0               | 0    |
| 1               | 4704 |
| 2               | 4736 |
| 3               | 4768 |
| 4               | 4800 |
| 5               | 4832 |
| 6               | 4864 |
| 7               | 4896 |
| 8               | 3328 |