TOOLS:
 * http://sami.on.eniten.com/hex2ip/
 * http://www.lammertbies.nl/comm/info/crc-calculation.html - CRC Calc Online, with XMODEM-CRC
 * http://base64decode.net - base64 decoder online
 * https://www.onlinedisassembler.com - Online Disassembler
 * http://hash.online-convert.com/crc32-generator - Online Hashing
 * http://www.md5.cz - MD5 Online
 * 
 
walkthrough:
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
  * Save all images to `./images/challenge1/`


