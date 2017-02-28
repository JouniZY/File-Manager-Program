##Wireless management programming

This program covers the basic wireless programming interface of the wireless memory device.
The URL requests related to the wireless function:

1. Get the detectable wireless access point information: 
`/svdo/WIFI{scan}`

After entering this URL in the browser. It will list down the information of all access points, as shown below:
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2010.59.34%20pm.png)

In the returned result of /svdo/WIFI{scan}, each line represents a detected wireles access point, ending with the symbol #.The information of each wireless access point is as follows:<br />
`<SSID>:<the signal intensity>:<whether encrypted>:<wireless mode>`

2. Get the current work status of the wireless memory device: 
`/svdo/WIFI{status}`<br />
The current work status returned by the wireless memory device is shown below:
![Image of Yaktocat]
(https://github.com/JouniZY/File-Manager-Program/blob/master/images/Screen%20Shot%202017-02-14%20at%2011.14.53%20pm.png)

The result of /svdo/WIFI{status} shows:<br />
Line 1, <wireless mode>:<battery status>:<number of active users>:<SSID> <br />
Line 2, <MAC address> <br />
Line 3, ending with the symbol # 

3. Set the wireless memory device to the point-to-point mode or Internet mode:<br />
`/svdo/WIFI{Adhoc}` :set the device to the point-to-point mode <br />
`/svdo/WIFI{status}` :set the device to the Internet mode <br/>
















```javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```
