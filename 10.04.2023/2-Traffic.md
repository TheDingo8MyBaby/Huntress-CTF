# Traffic:
![Traffic](https://i.imgur.com/aFQiV9I.png)
> We saw some communication to a sketchy site... here's an export of the network traffic. Can you track it down?  
>
>Some tools like [`rita`](https://github.com/activecm/rita) or [`zeek`](https://github.com/zeek/zeek) might help dig through all of this data!

**Download the file(s) below.**
**Attachments:** [traffic.7z](https://huntress.ctf.games/files/efd8115eedbda53848676208e38e6afc/traffic.7z?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjd9.ZR2FEA.Wp3SFO8FEL-TwzzyipWDy_sEUGQ)

-----

1. Download the *traffic.7z* file

2. Extract the folder within the file

3. Within the folder, you'll see multiple files ending in `.log.gz`

4. I then extracted all of the `.log.gz` files into a singular folder and extracted the contents of all of them
> 
	I chose to use `WinRar` for extracting the files

5. With all of the `.log` files extracted, I then used `CMD Prompt` to combine all of the `.log` files
> 

	copy *.log 1-combined.log

6. After CD'ing into the directory where all the `.log` files were located, I then ran the above command to combine all of the text files into 1 singular file.

7.  After I combined them, I then opened the file, and opened up the `find` dialog box within `notepad`
>
	I then searched for `"Sketchy"` and was directed to `sketchysite.github.io`
	
8.  Going to sketchysite.github.io will then bring you to this screen:

	![Sketchysite](https://i.imgur.com/Y0nrwcW.png)
  
10.  Flag #2 Acquired:

### flag{8626fe7dcd8d412a80d0b3f0e36afd4a}


-----

