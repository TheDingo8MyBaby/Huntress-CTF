# BaseFFFF+1:
![BaseFFF+1](https://i.imgur.com/7nVfY1Y.png)
> Maybe you already know about base64, but what if we took it up a notch?

**Download the file(s) below.**
**Attachments:** [baseffff1](https://huntress.ctf.games/files/4f5b4be19374471dc575e58e9c09637b/baseffff1?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjQ1fQ.ZR1pUQ.7c16x55Ncvb2UNdtxzl-pGCy1Ew)

-----

1. Download the *baseffff1* file

2. Open the `file` in a text editor
>

	I chose to use Notepad++

3. When you open it in your text editor, you'll see the following: `鹎驣𔔠𓁯噫谠啥鹭鵧啴陨驶𒄠陬驹啤鹷鵴𓈠𒁯ꔠ𐙡啹院驳啳驨驲挮售𖠰筆筆鸠啳樶栵愵欠樵樳昫鸠啳樶栵嘶谠ꍥ啬𐙡𔕹𖥡唬驨驲鸠啳𒁹𓁵鬠陬潧㸍㸍ꍦ鱡汻欱靡驣洸鬰渰汢饣汣根騸饤杦样椶𠌸`

4. If we look at the title of the challenge, we can tell that it's a Hexadecimal Representation
> -   F represents 15 in decimal.
> -   Therefore, "FFFF" represents 15 * 16^3 + 15 * 16^2 + 15 * 16^1 + 15 * 16^0 = 65535 in decimal.
> -   The '1' at the end represents 1 in decimal.

	So, "FFFF1" in hexadecimal is equivalent to 65536 in decimal.

5. We can then use a `Base65536` Decoder, such as: https://www.better-converter.com/Encoders-Decoders/Base65536-Decode
> 

	This will allow us to Decode the string found in Step#: 3

6. The output from the Decoded String will result in the below:

![Decoded](https://i.imgur.com/qv5UTU2.png)

7.  Flag #1 Acquired:

### flag{716abce880f09b7cdc7938eddf273648}


-----

