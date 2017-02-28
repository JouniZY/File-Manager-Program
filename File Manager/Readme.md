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

In the result of `</svdo/FSOpenDir{/fs/......}>`, each line represents a file or a subfolder, ending with the symbol #.<br/>
While the form is:<br/>
< file F/folder D >:< file size>:< file time>:< file name/folder name> <br/>

This program can only be operated on iPad.<br/>
Explanaiton of the JS funciton:<br/>
Variables:<br/>
```javascript
//Object for HTTP Request
var g_http_request = new XMLHttpRequest();
var g_http_async = new XMLHttpRequest();

//Touch operation
var l_touch_start_x;
var l_touch_start_y;
var l_touch_end_x;
var l_touch_end_y;
var l_clicked_item=9999;
var l_touch_moved;
var l_touch_id;

//File list
var g_left_folder;
var g_left_item_nbr;
var g_left_item_name = new Array();
var g_left_item_size = new Array();
var g_left_item_date = new Array();
var g_left_item_flag = new Array();
var g_left_item_select = new Array();
var g_left_item_handler = new Array();
var g_left_item_type = new Array();
var g_left_cell_height = 80;

//Screen size
var g_client_width = 320;
var g_client_height = 568;

```
Time conversion function that converts the file time to a format that is compatible with the current language:<br/>
```javascript
//Date time
var g_mon = new Array();
g_mon[0]="Jan.";
g_mon[1]="Feb.";
g_mon[2]="Mar.";
g_mon[3]="Apr.";
g_mon[4]="May.";
g_mon[5]="Jun.";
g_mon[6]="Jly.";
g_mon[7]="Aug.";
g_mon[8]="Sep.";
g_mon[9]="Oct.";
g_mon[10]="Nov.";
g_mon[11]="Dec.";
```
`DateTime2Str(datetime)`: This function converts the file time to a format that is wanted.<br/>
For instance, it converts "2011 3 23 21 10 0" to "Mar.23,2011 21:10:00".<br/>

Send the HTTP request to the wireless memory device and wait for the returned result: `HTTPGet(svdo_url)`<br/>

Open and read folders/files information from the storage device: `FSOpenDir()` <br/>

Calculate the file number clicked by the user according to the coordinate value: `CalculateClickedItem(touch_x, touch_y)`<br/>

Process the user clicks. If user clicks a folder, the function will enter the folder: `ProcessClickedItem(item_id)`<br/>

Touch screen operation function: `left_touchStart(event)`,`left_touchEnd(event)`,`left_touchMove(event)`.<br/>

Redraw file list function: `Draw_Panel()`.<br/>

Main function that is running while page loading: `main()`.<br/>



