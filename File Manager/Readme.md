##File Management Programming

This Chapter introduces the file management programming interface of the wireless memory device.<br/>
The URL request related to the file management function:

Open the root folder of the storage device and list down the contents in it: `</svdo/FSOpenDir{/fs/}>`<br/>
After entering the URL in the browser, the information of the root folder is shown:
![GitHub Logo]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.27.38%20pm.png)

Open another folder of the storage device that is connected to the wireless memory device and show its contents:<br/>
`</svdo/FSOpenDir{/fs/<folder name>/}>`<br/>
For instance, the following URL can be used to open the folder "SDK": `</svdo/FSOpenDir{/fs/C/My Videos/}>`<br/>
The execution result:
![GitHub Logo]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.28.09%20pm.png)



