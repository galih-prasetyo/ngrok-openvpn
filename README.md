<h1>THIS SCRIPT ONLY FOR EDUCATION</h1>
Please use this script wisely so it can be used forever.

How to make your own vpn server with ngrok and openvpn

Tool for use this script:

1. Github Account
2. (2) NGROK Account https://ngrok.io/
3. Putty(windows) / iTrem / Teminal (macOS & Linux)
4. Filezilla / other FTP Software

Follow this step for setup
  1. Sign in your github account https://github.com/
  2. Sign in first account Ngrok at https://dashboard.ngrok.com/ and second account ngrok at other window browser
  3. Remote your VPS with putty/terminal/iTerm & Login
  4. Use root
  >sudo su
  5. Input your vps password
  6. Install curl and openvpn
  >sudo apt update && apt install curl openvpn -y
  7. Get and install openvpn using
  >curl -O https://raw.githubusercontent.com/galih-prasetyo/ngrok-openvpn/main/openvpn-install.sh <br/>
  >chmod +x openvpn-install.sh <br/>
  >sudo ./openvpn-install.sh
  8. Here is my basic config. You can follow exact or customize to fill your need
  ``` 
  IP Address: 192.168.0.1                             (your local static ip address)
  Public IPv4 address or hostname: 0.tcp.ngrok.io     (or 1.tcp.ngrok.io)
  Do you want to enable IPv6 support (NAT)? [y/n]: n
  Port choice [1-3]: 1                                (Port 1194)
  Protocol [1-2]: 2                                   (TCP) (Ngrok only support TCP by default)
  DNS [1-12]: 9                                       (Google or anything for your taste)
  Enable compression? [y/n]: n
  Customize encryption settings? [y/n]: n             (default should suffice)
  Client name: galih                                   (anything suffice)
  Select an option [1-2]: 1                           (passwordless or password)
  ```
  9. Then you should receive file with .ovpn extension. Copy this file and keep it private
  10. In this case my file is nana.ovpn at /root/home/galih your filename and folder path might be different
  11. Download Ngrok binary from https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
  12. Unzip downloaded file to /home/galih (your user folder)
  13. Create new dir /home/galih/.ngrok2
  14. Create <b>ngrok.yml</b> file inside /home/galih/.ngrok2/
  ```
  authtoken: <YOUR AUTHTOKEN SECOND ACOOUNT NGROK>
  tunnels:
      openvpn:
          addr: 1194
          proto: tcp
  ```
  15. Change <YOUR AUTHTOKEN SECOND ACOOUNT NGROK> to your ngrok authtoken https://dashboard.ngrok.com/auth/your-authtoken
  16. Create service file called <b>ngrok.service</b> and place it in /etc/systemd/system/
  ```
  [Unit]
  Description=ngrok
  After=network.target

  [Service]
  ExecStart=/home/CHANGE-YOUR-NAME/ngrok start --all --config /home/CHANGE-YOUR-NAME/.ngrok2/ngrok.yml
  ExecReload=/bin/kill -HUP $MAINPID
  KillMode=process
  Restart=on-failure
  Type=simple

  [Install]
  WantedBy=multi-user.target
  ```
  17. Enable and start ngrok.service
  ```
  sudo systemctl enable ngrok.service
  sudo systemctl start ngrok.service
  ```
  18. To test if everything work correctly, do
  ```
  sudo systemctl status ngrok.service
  ```
  it should output something like
  ```
  ??? ngrok.service - ngrok
    Loaded: loaded (/etc/systemd/system/ngrok.service; enabled; vendor enabled)
    Active: active (running) since Mon 2020-08-24 04:35:22 UTC; 5 days ago
    Main PID: 682 (ngrok)
    Tasks: 11 (limit: 1677)
    Memory: 33.3M
    CGroup: /system.slice/ngrok.service
            ??????682 /home/galih/ngrok start --all --config /home/galih/.ngrok2/ngrok.yml
  ```
  ##SETTING VPN ON YOUR DEVICE##<br/>

  19. Install openvpn in your device (it could be your laptop or anything)
  20. Download your openvpn file with filezilla
  21. Open galih.ovpn file using your favorite text editor
  22. Change line to match tunnels url https://dashboard.ngrok.com/status/tunnels at your second ngrok account
  ```
    for example if your tunnel url is    tcp://1.tcp.ngrok.io:15342
    then change line to                  remote 1.tcp.ngrok.io 15342
  ```
  23. And that's it, now you're connected to your VPN
    
  
Have a good study

<h3>##USE THIS TUTORIAL WISELY##<h3>
