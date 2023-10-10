# Dumpster Fire?
![DumpsterFire?](https://i.imgur.com/ZD3Pc31.png)
>   We found all this data in the dumpster! Can you find anything interesting in here, like any cool passwords or anything? Check it out quick before the foxes get to it!  

**Download the file(s) below.**
**Attachments:** [dumpster_fire.tar.xz](https://huntress.ctf.games/files/d2b3f8dfd0c1b434f91b918080206d7e/dumpster_fire.tar.xz?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjMxfQ.ZSTZcg.1Yb1mmiedcePyun4PhYV3SXR5GE)

-----

1. Download the *dumpster_fire.tar.xz* file

2. When we open the *DataDump* in `7Zip`, we get a lot of directories. 


	2a. If we look into the `home` directory we can see a directory called **challenge**.

	2b. From there, you can go even deeper by going to `.mozilla` -> `firefox` -> `bc1m1zlr.default-release` -> 


	2c.  Once you're deep enough, you should see a `.json` file:  `logins.json` along with a `.db` file: `key4.db`

	![enter image description here](https://i.imgur.com/X7TQM1V.png)


3. If you were to open `logins.json` and run it through a quite `JSON Beautifier (CyberChef)`, you'll get a code like this.

- 3a. The JSON Data:
```
{

-   nextId: 2,
-   logins: [
    
    1.  {
        
        -   id: 1,
        -   hostname: [http://localhost:31337](http://localhost:31337),
        -   httpRealm: null,
        -   formSubmitURL: [http://localhost:31337](http://localhost:31337),
        -   usernameField: "username",
        -   passwordField: "password",
        -   encryptedUsername: "MDIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECPs50spbp6eyBAi0aCUHIntLPA==",
        -   encryptedPassword: "MFIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECEcjS+e6bXjFBCgCQ0p/1wCqPUmdgXdZWlohMXan4C3jD0bQgzsweyVEpAjJa+P9eOU4",
        -   guid: "{9a363712-620c-499a-bb7d-999b8b2515dc}",
        -   encType: 1,
        -   timeCreated: 1604703907434,
        -   timeLastUsed: 1604703907434,
        -   timePasswordChanged: 1604703907434,
        -   timesUsed: 1
        
        }
    
    ],
-   potentiallyVulnerablePasswords: [],
-   dismissedBreachAlertsByLoginGUID: {},
-   version: 3

}
```

5. We can see that the password is currently Encrypted: 
```MFIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECEcjS+e6bXjFBCgCQ0p/1wCqPUmdgXdZWlohMXan4C3jD0bQgzsweyVEpAjJa+P9eOU4```

7. In order to decrypt the password, we can now take the `key4.db` file and the `logins.json` file and then them through [firepwd](https://github.com/lclevy/firepwd)
	![FirePWD](https://i.imgur.com/5ATJNV9.png)
8. After running the two files through `firepwd`, we can see that the decrypted password ends up being the flag that we were looking for
 
9. Flag #2 Acquired:

### flag{35446041dc161cf5c9c325a3d28af3e3}

-----

