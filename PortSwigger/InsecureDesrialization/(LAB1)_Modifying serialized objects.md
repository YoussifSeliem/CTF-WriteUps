# Modifying serialized objects
link: https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects

This lab uses a serialization-based session mechanism and is vulnerable to privilege escalation as a result. To solve the lab, edit the serialized object in the session cookie to exploit this vulnerability and gain administrative privileges. Then, delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

### solution
- start by logging in using **wiener:peter** creds

<img src=./images/Capture0.PNG alt="img0" width="700"/>

- We see that change email functionality
- let's use it and intercept the request to see if there's an interesting thing hidden

<img src=./images/Capture1.PNG alt="img1" width="700"/>

- we note the session cookie may be interesting: `Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d`
- go to cyberchef and decode it

<img src=./images/Capture2.PNG alt="img2" width="700"/>

- The value after decoding is: `O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}`
- after examining it carefully we find the attribute **admin** with binary value = **false**
- make it true by making the string `O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}`
- then encode it again to be `Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30%3D`
- send the request again but make sure to change the value of the cookie with the new value
- you will find a new functionality for the user wiener which is **admin panel**

<img src=./images/Capture3.PNG alt="img3" width="700"/>

- access it and you will find users list

<img src=./images/Capture4.PNG alt="img4" width="700"/>

- Delete the user carlos
- Congratzzzzzzzzzzzzzzzzz