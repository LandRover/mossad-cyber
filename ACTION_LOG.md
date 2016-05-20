TOOLS:
 * http://sami.on.eniten.com/hex2ip/
 * http://www.lammertbies.nl/comm/info/crc-calculation.html - CRC Calc Online, with XMODEM-CRC
 * http://base64decode.net - base64 decoder online
 * https://www.onlinedisassembler.com - Online Disassembler
 * http://hash.online-convert.com/crc32-generator - Online Hashing
 * http://www.md5.cz - MD5 Online
 * https://mh-nexus.de/en/ - HxD for Windows
 
Walkthrough:
 1. `82d354aa` extracted from image -> HEX2IP -> 130.211.84.170
 2. Open http://130.211.84.170 -> http://130.211.84.170/challenge1/login
 3. Save the LOCK image at the left side bar to ./images/challenge1/logo.png
  * Image URL: http://130.211.84.170/challenge1/get-image?name=logo.png&h=87d41d15f&multiple=0
 4. CRC Calculator decode for `87d41d15f` taken from the URL returns `0x2BAD` (funny but hints probably that it is irrelevant or a lucky match)
 5. Convert get param `logo.png` to MD5 `1bb87d41d15fe27b500a4bfcde01bb0e`, notice `87d41d15f` begins at char index 3 -> .substr(3, 9)
 6. Take the wildcard string `*` to MD5 `3389dae361af79b04c9c8e7057f60cc6`.substr(3, 9) -> `9dae361af`
 7. Take the original image URL and set the params and open http://130.211.84.170/challenge1/get-image?name=*&h=9dae361af&multiple=1
  * Params:
   * name=* (filename)
   * h=9dae361af (md5 checksum)
   * multiple=1 (returns array rather than a single file)
 8. URL returns a file JSON list of files: ["last-login.png", "map.png", "themissingpiece.png", "logo.png", "a.jpg", "fingerprint.jpg"]
 9. Convert to MD5 all file names:
   * `last-login.png` -> `52704f70b70a56d9ec119bd6cb2fc69c`.substr(3, 9) -> `04f70b70a`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=last-login.png&h=04f70b70a&multiple=0
   * `map.png` -> `92a36320069a227bd1685020739dab61`.substr(3, 9) -> `36320069a`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=map.png&h=36320069a&multiple=0
   * `themissingpiece.png` -> `6bf67f30c14f1df6219c683f3efc4c45`.substr(3, 9) -> `67f30c14f`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=themissingpiece.png&h=67f30c14f&multiple=0
   * `logo.png` -> `1bb87d41d15fe27b500a4bfcde01bb0e`.substr(3, 9) -> `87d41d15f`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=logo.png&h=87d41d15f&multiple=0
   * `a.jpg` -> `394659692a460258b45a99f1424ea357`.substr(3, 9) -> `659692a46`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=a.jpg&h=659692a46&multiple=0
   * `fingerprint.jpg` -> `6ee84f3db18deca2624ff8d39b5fb2d6`.substr(3, 9) -> `84f3db18d`
    * Image URL: http://130.211.84.170/challenge1/get-image?name=fingerprint.jpg&h=84f3db18d&multiple=0
 10. Save all images to `./images/challenge1/`
 11. Upload the image `last-login.png`
  * Returns an error: `User is already logged in. Try another token or contact support.`
 12. Check the XMODEM-CRC for `./images/challenge1/last-login-04f70b70a.png` -> `398e`
'''
var crc = require('crc'),
    fs = require('fs');

var out = crc.crc16xmodem(fs.readFileSync('last-login.png')).toString(16);

console.log(out);
'''
 13. Use the python to generate fake PNG header with the XMODEM-CRC === `398e`
 14. Login!
 15. Notice the new menu options:
  * Cells
  * Security Cameras
  * Door Control
  * Map
  * Logout
 16. Enter Door Control and take The base64 and decode it:
  * `UEsDBBQAAAAIAG93rUhABqejpQMAAAAKAAAPAAAARG9vckNvbnRyb2wuZXhl842awMDMwMDAAsT//zMw7GCAAAcGwuAAEPPJ7+Jj2MJ5VnEHo89ZxZCMzGKFgqL89KLEXIXkxLy8/BKFpFSFotI8hcw8BRf/YIXc/JRUPV5eLhWoGTsrlQ/+lfCdBMMKz9sm/QOzfSb9ANINiWsn/QHTk8F0UGZyBkgdzA0BrgwMPozMDPxr7oXBxB4wMDFyM/IwMDBBPQYCAlCsAPUdiA2UZ4NKw2iwx6F6mBkcWsEKBaAGCCCMgoE/QPM08AWSAQODDBFhiQGA5krgkdYrSa0oAdITGBngfoH7FWFEgl5RSmJJIgPDFEaIAFgdG6o6ByDSK0rN​yU+GutUAqo4DQ50TOV4ZBYMXhHa/aXxjAEwcwWHhWayR+yQUHBh6ax98XrpPB8w6k6UaaXy7tzXz////S7OSgwOWdszNALGXLtnnAFKx98K///8/L01buuSFBNCY5iM83YcbSlgf3HBo/iFc8zU4OIs5ONj4W6/vA7ewwP+iDEA9zT/+87c8Aloe3Ov7IzBLsLcXZGJgwH9RFqBs6wH+lqNAyebaHwL8rbuBrO7TXS68Z6xaeEHK+FtXAIUcm38​KFL3s/tywxRRk/6oSTrfGf9OBEkVvu20FQO5y/RIckCWccQPIzvr6/3q3LA+QFfD/chZQ6kdAFnOv65+ArG//rwNt5QDbWspqfMDhdQDQWubSrw21f4xK3zfU/rUofdnl+k/HwIaz/AFI65eALN6MN1BDgcY11P4zLBWFyIT1toIcEwCVMj7wmqn7WHxcdPfT2MPAcO725eFf7yvQerJEtPui8​YHw7lqOgy+ZPq/uPtX8kPnzqvhuV47Yw/SOfx9FBoYoIE4D4jIgtlGEiO9+1LJn+/ZVq1t2b9+2b+P+b2sWrGFItorxL0jNc8nPLyrWy06tBKk7dO/erQmtrbNnzmqZMXt2y6R5LbPnzmo9eej0sWPnj5w9P7v15v2TJ8/fvNk68/y5E7Pmnr//9fTlO7d/zr1/4+HB+ycuH7z/6/TcOzcuz78/t33unbkz5t561j551W5KAQODa15JapFCcmpOjkJeaW5SapGVAgPI4QogH6SmcHEB3a6iiAiHNhBbgbQw​OsDknloSXJLikZiXkpPKwLCLwbkoNbEk1S0zJ9WRIYAlKDUxBcRmYHABs53z84rzQVIM11jDizJLUuEC​3q5Bfq4+xkZ6KTk5NItq7EAAUuYLGEgZ2BgUGPQZXDF4YPDKgMXQ1JDOLhkFAwAAUEsBAhQDFAAAAAgA​b3etSEAGp6OlAwAAAAoAAA8AAAAAAAAAAAAAAIABAAAAAERvb3JDb250cm9sLmV4ZVBLBQYAAAAAAQAB​AD0AAADSAwAAAAA=`
 17. Take the decoded Snipit:
  * `PKowH@
DoorControl.exe30`cyVqYŐb\ļT<<`T=^^.;+
&}&
k'ӓtPfrH
>k001r3000A=P
gJhǡzZ
 ?@4aJ+I(~aD^QJbI"FX: +JOC9^vi|cLaY$zk|^O:i|5Kv.R¿?/M[И#<݇JXph!\58898[@=?<Z#0Kdb`Ql@].gZxA[W
ܰdNӁEom@r%qz,rPG@s럀omZj|uZү
J7(}OH뗀,ތ7PC53,Ȅ2>X|\tp_+zDZ/>>nWE( N2 Q~ԲgU[vo߶ookaH/Hs/*N;tޭ	gj1{vˤy-j=yc珜=?'߼:枿;ν'.;7.Ͽ?}3z>ynJk^IjBrjNB^inRj
 pqݮ6[0:Z\⑘(5$-3'Ց!%(51f`pARXË2KRޮA~>Fz)994j@RH\1x`ʀԐ.PKowH@
DoorControl.exePK=` 


