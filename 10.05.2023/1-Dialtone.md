# Dialtone
![DialTone](https://i.imgur.com/sb7QFjD.png)
> Well would you listen to those notes, that must be some long phone number or something!  

**Download the file(s) below.**
**Attachments:** [dialtone.wav](https://huntress.ctf.games/files/f45233d4c250e2f75e5aae03725fffc7/dialtone.wav?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjEzfQ.ZSMHpw.x58ouyd82m3e_R4z8AAOuAy2gJE)

-----

1. Download the *dialtone.wav* file

2. If you open one of the files in a `DTMF Decoder` online, such as `https://dtmf.netlify.app/`, you'll a text output like this:
```13040004482820197714705083053746380382743933853520408575731743622366387462228661894777288573```

3. I then did a bit of `GoogleFu` and came across a StackedOverflow post about `Convert big number to chars - Python`.  


	3a. https://stackoverflow.com/questions/41123816/convert-big-number-to-chars-python

4.   I then modified a simple `.py` script, to convert the number output from the DTMF:

- 4a. This was the Python Script
```
import codecs plainInt =
13040004482820197714705083053746380382743933853520408575731743622366387462228661894777288573
b = hex(plainInt).rstrip("L").lstrip("0x") print(codecs.decode(b, 'hex').decode('utf-8'))
```

![enter image description here](https://i.imgur.com/QfdJjY3.png)

5. Flag #1 Acquired:

### flag{6c733ef09bc4f2a4313ff63087e25d67}

-----

