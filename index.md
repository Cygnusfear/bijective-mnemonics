In cryptography and related fields, public and private keys consist of >40
hexadecimal digits, such as  `0x85b463314d8177fdb2a590c6af321699e2d718cc`.
When dealing with a lot of these, for example when trying to read transaction
details in a block of a blockchain, discerning addresses can become a bit of a
hassle. As a demonstration, you are invited to filter out keys that appear 2 or
more times in the following list:

```
0x85b463314d8177fdb2a590c6af321699e2d718cc
0x16b0c89b6c987fc1687c6c5d5f19f9f0543f2ba7
0xf054e6f785cbf6ca764d0bad1dcf47e12c070484
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f
0x570436a62b6e5d1b54ec3d3ab9c21a979bf8dc2b
0xba6cf19a5fc9277f4e976b41a7789adc8cd1fefd
0x466b5222d47b8533f15a32ef9f82a3a9fd6f8b2f
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f
0x077be25134037a10160e6f4bded4c17a4765508a
0x65ab6b69c47815b6bd89327ab67e19675212ad4a
0x16b0c89b6c987fc1687c6c5d5f19f9f0543f2ba7
0x20f81043f12dde4440ecc0b35156a1275f181653
0xba6cf19a5fc9277f4e976b41a7789adc8cd1fefd
0x077be25134037a10160e6f4bded4c17a4765508a
0x65ab6b69c47815b6bd89327ab67e19675212ad4a
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f
```

It gives you a headache, doesn't it?

As a solution, we can use a scheme that is similar to
[BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) in
order to map any hexadecimal number---including private, public keys, addresses,
etc.---to a mnemonic phrase, e.g.

```
0x85b463314d8177fdb2a590c6af321699e2d718cc
<=>
aerobic home shoe below scheme that rent pitch mail profit goddess hat vapor fragile book
```

Why not just use BIP39? Because
[BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) together
with related
[BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) called
Hierarchical Deterministic Wallets
is a scheme designed to *generate* multiple keys from
*randomly selected* seed phrases. In HDM's, trapdoor functions are used to
obtain keys from the seed phrase, making it infeasible to map back to the
mnemonic phrase, given the key.

What we desire is a bijection where mapping from mnemonic phrase to key and vice
versa don't cost anything. Notice that we don't use the word 'seed', because
this scheme is not used for generating multiple keys, but having an invertible,
one-to-one and onto map.

To this end, we use the
[2048 word long lists from BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039/bip-0039-wordlists.md)
as digits of a base-2048 numbering system. We assemble phrases from base-16 keys
the same way we do any base conversion, but this time, replacing symbols with
words and putting spaces in between. It's that simple!

The word list for English goes like this: *abandon*, *ability*, *able*... So for
`0x1`, we have `ability`, for `0x800`, we have `ability abandon`,
and for `0x400000`, we have `ability abandon abandon`. This is like writing 1,
10, 100, but for base-2048.

To increase readability, we can shorten phrases by cutting
out portions. Notice how the demonstration above gets easier when we lay beside
the first and last words of corresponding mnemonic phrases:

```
0x85b463314d8177fdb2a590c6af321699e2d718cc <=> aerobic..book
0x16b0c89b6c987fc1687c6c5d5f19f9f0543f2ba7 <=> absent..insane
0xf054e6f785cbf6ca764d0bad1dcf47e12c070484 <=> always..mountain
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f <=> album..program
0x570436a62b6e5d1b54ec3d3ab9c21a979bf8dc2b <=> actor..lyrics
0xba6cf19a5fc9277f4e976b41a7789adc8cd1fefd <=> alarm..text
0x466b5222d47b8533f15a32ef9f82a3a9fd6f8b2f <=> acquire..grass
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f <=> album..program
0x077be25134037a10160e6f4bded4c17a4765508a <=> ability..bacon
0x65ab6b69c47815b6bd89327ab67e19675212ad4a <=> add..power
0x16b0c89b6c987fc1687c6c5d5f19f9f0543f2ba7 <=> absent..insane
0x20f81043f12dde4440ecc0b35156a1275f181653 <=> absurd..skill
0xba6cf19a5fc9277f4e976b41a7789adc8cd1fefd <=> alarm..text
0x077be25134037a10160e6f4bded4c17a4765508a <=> ability..bacon
0x65ab6b69c47815b6bd89327ab67e19675212ad4a <=> add..power
0xbfdfaef8656a810adb72d9ee2b30bd0e15aa0d5f <=> album..program
```

This trades off uniqueness against readability and memorability, and introduces
the risk of colliding phrases.

<style>
input, textarea {
    width: 100%;
    text-align: center;
    font-size: 110%;
    font-size: 110%;
    font-family: monospace;
    padding: 1ex;
}
textarea {
    resize: horizontal;
    vertical-align: middle;
}
</style>

## Hex to Mnemonic

### Enter Hexadecimal Number:

<input type="text" id="hexinput" value="0x85b463314d8177fdb2a590c6af321699e2d718cc">

### Corresponding Bijective Mnemonic Phrase:

<textarea type="text" id="mnemonicoutput" rows="4" readonly></textarea>


## Mnemonic to Hex

### Enter Mnemonic Phrase:

<textarea type="text" id="mnemonicinput" rows="4">aerobic home shoe below scheme that rent pitch mail profit goddess hat vapor fragile book</textarea>

### Corresponding Hexadecimal Number

<input type="text" id="hexoutput" readonly>


<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.slim.js"></script>
<script src="{{ '/BigInteger.min.js' | prepend: site.baseurl | prepend: site.url }}"></script>
<script src="{{ '/mnemonic.js' | prepend: site.baseurl | prepend: site.url }}"></script>

<script>
function update_hex_to_mnemonic() {
   var hex_str = $("#hexinput").val();
   try {
      var mnemonic_phrase = addr_to_mnemonic(hex_str);
      $("#mnemonicoutput").val(mnemonic_phrase);
   } catch {
      $("#mnemonicoutput").val('Invalid input');
   }
}

function update_mnemonic_to_hex() {
   var mnemonic_str = $("#mnemonicinput").val();
   try {
       var mnemonic_phrase = mnemonic_to_addr(mnemonic_str);
       $("#hexoutput").val(mnemonic_phrase);
   } catch {
       $("#hexoutput").val('Invalid input');
   }
}


$("#hexinput").on("keyup change load", function() {update_hex_to_mnemonic();});
$("#mnemonicinput").on("keyup change load", function() {update_mnemonic_to_hex();});

$(document).ready(function(){
  update_hex_to_mnemonic();
  update_mnemonic_to_hex();
});

</script>















