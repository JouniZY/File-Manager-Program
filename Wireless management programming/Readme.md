##Wireless management programming

This program covers the basic wireless programming interface of the wireless memory device.

There is a basic function:<br/>
HTTPsvdo(svdo_url):<br/>
This function will convert the request sent to the device to an indentifiable HTTP URL and then send it to the wireless memory device. After that, the function will wait for the return of the device execution result, which is a text string.<br/>


The URL requests related to the wireless function:<br/>

Get the detectable wireless access point information: 
`/svdo/WIFI{scan}`

After entering this URL in the browser. It will list down the information of all access points, as shown below:
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2010.59.34%20pm.png)

In the returned result of /svdo/WIFI{scan}, each line represents a detected wireles access point, ending with the symbol #.The information of each wireless access point is as follows:<br />
`<SSID>:<the signal intensity>:<whether encrypted>:<wireless mode>`

Get the current work status of the wireless memory device: <br/>
`/svdo/WIFI{status}`<br />
The current work status returned by the wireless memory device is shown below:
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.14.53%20pm.png)

The result of /svdo/WIFI{status} shows:<br />
Line 1, < wireless mode>:< battery status>:< number of active users>:< SSID> <br />
Line 2, <  MAC address> <br />
Line 3, ending with the symbol # 

Set the wireless memory device to the point-to-point mode or Internet mode:<br />
`/svdo/WIFI{Adhoc}` :set the device to the point-to-point mode <br />
`/svdo/WIFI{status}` :set the device to the Internet mode <br/>
The wireless device will return "ok" after the mode configuration is successful:
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.18.20%20pm.png)

Power off the wireless memory device: `/svdo/EXIT{1}`<br />

Explanation of the example of the wireless programming interface:<br />
Scan Access Point<br />
```javascript
//Define an array to save the wireless sccess point information
var g_ap_info_ssid = new Array();
var g_ap_info_signal = new Array();
var g_ap_info_encrypt = new Array();
var g_ap_info_mode = new Array();
var g_ap_number;
//Convert the wireless access point information stored in the array into an HTML table and output this table
function output_ap_info()
{
 var i;
 
 document.write("Scan Result:");
 document.write("<table border=\"1\"");
 document.write("<tr >"); 
 document.write("<th >"+"ESSID"+"</th>");
 document.write("<th >"+"Quality"+"</th>");
 document.write("<th >"+"Encrypt"+"</th>");  
 document.write("<th >"+"Mode"+"</th>");  
 document.write("</tr>"); 	 	
 for ( i=0; i<g_ap_number; i++)
 		{
 		document.write("<tr >");
  	document.write("<td >"+g_ap_info_ssid[i]+"</td>");
  	document.write("<td >"+g_ap_info_signal[i]+"</td>");
  	document.write("<td >"+g_ap_info_encrypt[i]+"</td>");  	
  	document.write("<td >"+g_ap_info_mode[i]+"</td>"); 
  	document.write("</tr>");
		}
 document.write("</table>");
}
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
//Get the main function of the wireless access point
function wifi_scan()
{
	var result;
	var i;
	var scan_len;
	var scan_posi;
	var scan_end;
 
 //Clear all informations
	g_ap_info_ssid.splice(1);
	g_ap_info_signal.splice(1);
	g_ap_info_encrypt.splice(1);
	g_ap_info_mode.splice(1);
	g_ap_number=0;
 
 //Send command to CloudFTP
 result=HTTPsvdo("WIFI{scan}");
 if(result==null)
 		{
		alert("Fail to scan APs!");
		return(0);
		}
		
 //4,Process items
 scan_len=result.length;
 scan_posi=0;
 while(scan_posi<scan_len)
		{
		if(result.charAt(scan_posi)=='#')
			{
			break;
			}
		//Pickup SSID
		scan_end = result.indexOf(":",scan_posi);
		if(scan_end == -1)
			break;
		g_ap_info_ssid[g_ap_number] = result.slice(scan_posi,scan_end);
		scan_posi = scan_end+1;
		//Pickup Signal
		scan_end = result.indexOf(":",scan_posi);
		if(scan_end == -1)
			break;
		g_ap_info_signal[g_ap_number] = result.slice(scan_posi,scan_end);
		scan_posi = scan_end+1;
		//Pickup encrypt
		scan_end = result.indexOf(":",scan_posi);
		if(scan_end == -1)
			break;
		g_ap_info_encrypt[g_ap_number] = result.slice(scan_posi,scan_end);
		scan_posi = scan_end+1;
		//Pickup mode
		scan_end = result.indexOf("\n",scan_posi);
		if(scan_end == -1)
			break;
		g_ap_info_mode[g_ap_number] = result.slice(scan_posi,scan_end);
		scan_posi = scan_end+1;
		//Next item
		g_ap_number++;
		}
		
	//Check result
	if(g_ap_number==0)
		{
		alert("Access Point not found!");
		return(0);
		}
	return(g_ap_number);
}
```
Program execution result
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-15%20at%2012.07.17%20am.png)

Get the current work status of the wireless device<br/>
```javascript
var g_client_ip = new Array();
var g_client_number;
var g_current_mode;
var g_battery_status;
var g_current_ssid;

function output_client()
{
 var i;
 
 document.write("Clients IP Address:");
 document.write("<table border=\"1\"");
 document.write("<tr >"); 
 document.write("<th >"+"No."+"</th>");
 document.write("<th >"+"IP Address"+"</th>");
 document.write("</tr>"); 	 	
 for ( i=0; i<g_client_number; i++)
 		{
 		document.write("<tr >");
  	document.write("<td >"+(i+1)+"</td>");
  	document.write("<td >"+g_client_ip[i]+"</td>");
  	document.write("</tr>");
		}
 document.write("</table>");
}

function wifi_status()
{
	var result;
	var i;
	var scan_len;
	var scan_posi;
	var scan_end;
 
 //Clear all informations
	g_client_ip.splice(1);
	g_client_number=0;
 
 //Send command to CloudFTP
 result=HTTPsvdo("WIFI{status}");
 if(result==null)
 		{
		alert("Fail to obtian status!");
		return(0);
		}
		
 //4,Process items
 scan_len=result.length;
 scan_posi=0;
 
 //Pickup Current Mode
 scan_end = result.indexOf(":",scan_posi);
 if(scan_end == -1)
		return(0);
 g_current_mode = result.slice(scan_posi,scan_end);
 scan_posi = scan_end+1;

 //Pickup Battery Status
 scan_end = result.indexOf(":",scan_posi);
 if(scan_end == -1)
		return(0);
 g_battery_status = result.slice(scan_posi,scan_end);
 scan_posi = scan_end+1;

 //Pickup Client Number
 scan_end = result.indexOf(":",scan_posi);
 if(scan_end == -1)
		return(0);
 g_client_number = result.slice(scan_posi,scan_end);
 scan_posi = scan_end+1;

 //Pickup Current SSID
 scan_end = result.indexOf("\n",scan_posi);
 if(scan_end == -1)
		return(0);
 g_current_ssid = result.slice(scan_posi,scan_end);
 scan_posi = scan_end+1;
 
 alert("Current SSID: "+g_current_ssid);
 i=0;
 while(scan_posi<scan_len)
		{
		if(result.charAt(scan_posi)=='#')
			{
			break;
			}
		//Pickup Client IP
		scan_end = result.indexOf("\n",scan_posi);
		if(scan_end == -1)
			break;
		g_client_ip[i] = result.slice(scan_posi,scan_end);
		scan_posi = scan_end+1;
		//Next item
		i++;
		}
		
	//Check result
	if(i==0)
		{
		alert("Client not found!");
		return(0);
		}
	return(i);
}
```
Program execution result <br/>
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-15%20at%2012.11.52%20am.png)
Switch the work mode of the wireless memory device to the point-to-point mode. Switch to Adhoc mode<br/>
``` javascript
function wifi_switch_adhoc()
{
 if( HTTPsvdo("WIFI{Adhoc}")=="ok\n")
 		alert("Switch to Ad-hoc mode successfully!");
 else
 	  alert("Fail to switch to 'Ad-hoc' mode!");
}
```
Program execution result <br/>
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.18.20%20pm.png)

Switch the work mode of the wireless memory device to the Internet mode<br/>
``` javascript
function wifi_switch_infra()
{
 var result;
 
 result=HTTPsvdo("WIFI{Infra}");
 if( result=="ok\n")
 		alert("Switch to Infra mode successfully!");
 else
 	  alert("Fail to switch to 'Infra' mode!");
}
```
Program execution result <br/>
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-15%20at%2012.07.53%20am.png)
Power off the device
```javascript
function power_off()
{
 HTTPsvdo("EXIT{1}");
 alert("'Power Off' command was send!");
}
```


















