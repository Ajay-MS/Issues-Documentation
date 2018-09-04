## WinRM Troubleshoot

#### WinRM
WinRM (Windows Remote Management) is a remote management service.

Both client and server computer should have winRM installed and enable in order to establish remote connection. 

#### Configure WinRM on remote server

Open CMD and run following command
```
winrm quickconfig
```
It performs following operations
* Start WimRM service. 
* Loads default WimRM configuration. 
* Sets service startup type to auto-start. 
* Configure listener port that exchanges WS-Management protocol messages using HTTP/HTTPs.

#### Verify if WinRM listener is running on remote server

Open CMD and run one of following command
```
winrm enumerate winrm/config/listener
```

OR

```
winrm e winrm/config/listener
```

OR

```
winrm get winrm/config
```

Also, using above mentioned command you can identify port on which WinRM service is running. 
* Port 5985  - HTTP
* Port 5986  - HTTPS
