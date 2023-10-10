# Where Am I?
![WhereAmI?](https://i.imgur.com/l7oJJF9.png)
> Your friend thought using a JPG was a great way to remember how to login to their private server. Can you find the flag?  

**Download the file(s) below.**
**Attachments:** [PXL_20230922_231845140_2.jpg](https://huntress.ctf.games/files/c0d504cd54b9e52ba752bb7a32503a89/PXL_20230922_231845140_2.jpg?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjI4fQ.ZSTUOg.1BGFF-l2RPMM-HunEnfWGMUe5E8)

-----

1. Download the *PXL_20230922_231845140_2.jpg* file

2. Because this challenge mentioned OSINT, I immediately thought that the image had to have something hidden within the MetaData.

3. I decided to open the image in a MetaData Extractor, to see what information I could obtain from it..  


	3a. https://brandfolder.com/workbench/extract-metadata

4.   After reviewing the details, I noticed a text string that looked just like a common Base64 Encoding:

- 4a. The TextString was:
```
ImageDescription: 
ZmxhZ3tiMTFhM2YwZWY0YmMxNzBiYTk0MDljMDc3MzU1YmJhMik=
```

![OSINT](https://i.imgur.com/QGIj1rp.png)

5. After decoding the Base64. I was then given the flag!

6. Flag #1 Acquired:

### flag{b11a3f0ef4bc170ba9409c077355bba2)

-----

