TOOLS:
 * http://sami.on.eniten.com/hex2ip/
 * http://www.lammertbies.nl/comm/info/crc-calculation.html - CRC Calc Online, with XMODEM-CRC
 * http://base64decode.net - base64 decoder online
 * https://www.onlinedisassembler.com - Online Disassembler
 * http://hash.online-convert.com/crc32-generator - Online Hashing
 * http://www.md5.cz - MD5 Online
 * 
 
Walktrough:
 1. `82d354aa` extracted from image -> HEX2IP -> 130.211.84.170
 2. Open http://130.211.84.170 -> http://130.211.84.170/challenge1/login
 3. Save the LOCK image at the left side bar to ./images/challenge1/logo.png
  * Image URL: http://130.211.84.170/challenge1/get-image?name=logo.png&h=87d41d15f&multiple=0
 4. CRC Calculator decode for `87d41d15f` taken from the URL returns `0x2BAD` (funny but hints probably that it is irrelevant or a lucky match)
 5. 
