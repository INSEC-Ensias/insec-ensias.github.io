---
title: "CYBERTALENTS — Web Security CTF"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - ctf
  - writeup
---

![image](https://github.com/INSEC-Ensias/insec-ensias.github.io/blob/main/assets/images/0_RE7m7FMN9NI5lA4g.jpg?raw=true)
Hello guys, Cybertalents organized two ctfs for penetration testing scholarship and i liked to introduce you to a new vulnerability *“insecure deserialization”*.

For our challenge, we have a serialized object that we can exploit for editing the cookie.
-Read more about this vulnerability here:-[https://portswigger.net/web-security/deserialization](https://portswigger.net/web-security/deserialization) 

This challenge consists of a simple web page :
![image](https://github.com/INSEC-Ensias/insec-ensias.github.io/blob/main/assets/images/1_9RKhE0QfcEHn_2uSEwpgNQ.png?raw=true)
I first intercepted the requests using Burp and then i found this cookie that seemed like base64 encoded :
userCookie=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJOYW1lIjtzOjk6ImFub255bW91cyI7czo3OiJpc0FkbWluIjtiOjA7fQ%3D%3D
after decoding we’ve got a JSON thing, that is actually the serialized object :
![image](https://github.com/INSEC-Ensias/insec-ensias.github.io/blob/main/assets/images/1_tcearRbc3E_1uR0Drd98uw.png?raw=true)
####O:4:”User”:2:{s:8:”userName”;s:9:”anonymous”;s:7:”isAdmin”;b:0;}
“O” is used for an object in front of it we find the number of elements, and “s” is the string format, in case we find “i” it means integer and “b” you know.. insecure deserialization allows the attacker to change attributes to take privilege if stored in cookies.
As the attacker can manipulate this data i thought of changing the roles,
i tried to modify the “anonymous” to “admin” which corresponds to “userName” making attention to the string count “s”, and changing “b” which is boolean to 1.
O:4:”User”:2:{s:8:”userName”;s:5:”admin”;s:7:”isAdmin”;b:1;}
All i’ve done was encoding this JSON to base64 again and paste it in the cookie value:
![image](https://github.com/INSEC-Ensias/insec-ensias.github.io/blob/main/assets/images/1_9cyiaWiKrcfjbPURrtpAVw.png?raw=true)
Our request was successfull, we the find the flag.
Thank you for reading till the end, for further information about this vulnerability you can find this good lab:-[https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects) 

—
Twitter:- [https://twitter.com/4rr4ys](https://twitter.com/4rr4ys)

### Club Links:
Facebook:- [https://www.facebook.com/ensiasinsec](https://www.facebook.com/ensiasinsec)
Youtube:- [https://www.youtube.com/channel/UCQ_DuJJtTg05hjntfwVEy0Q](https://www.youtube.com/channel/UCQ_DuJJtTg05hjntfwVEy0Q)
Instagram:- [https://www.instagram.com/insec_club/](https://www.instagram.com/insec_club/)
Linkedin:- [https://www.linkedin.com/company/insec-ensias/](https://www.linkedin.com/company/insec-ensias/)
writer: [Ikram Bourhim](https://medium.com/@ikramsofiasimone)