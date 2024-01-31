# Modifying serialized data types
link: https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-data-types

 This lab uses a serialization-based session mechanism and is vulnerable to authentication bypass as a result. To solve the lab, edit the serialized object in the session cookie to access the administrator account. Then, delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter 

### solution
- start by logging in using **wiener:peter** creds

<img src=./images/Capture0.PNG alt="img0" width="700"/>

- We see that change email functionality
- let's use it and intercept the request to see if there's an interesting thing hidden

<img src=./images/Capture5.PNG alt="img5" width="700"/>

- we note the session cookie may be interesting: `Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJreWF3MmFrd2t4dDBheDF1bGNwZWk4ZzhhYjFiNnp2eiI7fQ%3d%3d`
- go to cyberchef and decode it

<img src=./images/Capture6.PNG alt="img6" width="700"/>

- The value after decoding is: `O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"kyaw2akwkxt0ax1ulcpei8g8ab1b6zvz";}`
- after examining it carefully we find the attribute **access_token** with string value which may be compared againest other string
- so let's try to make it integer with value 0 and if the comparison is loose comparison the condition will be true
- change it to `O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";i:0;}`
- then encode it again to be `Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtpOjA7fQ%3D%3D`
- send the request again but make sure to change the value of the cookie with the new value
- you will find a new functionality for the user wiener which is **admin panel**

<img src=./images/Capture3.PNG alt="img3" width="700"/>

- access it and you will find users list

<img src=./images/Capture4.PNG alt="img4" width="700"/>

- Delete the user carlos
- Congratzzzzzzzzzzzzzzzzz