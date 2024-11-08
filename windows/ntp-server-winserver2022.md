```
@echo off
echo Setting up NTP Server on Windows Server 2022...

:: Start Windows Time service
echo Starting Windows Time service...
net start w32time

:: Configure NTP server settings
echo Configuring NTP server settings...
w32tm /config /syncfromflags:MANUAL /manualpeerlist:"time.windows.com" /reliable:YES /update

:: Enable NTP server mode in registry
echo Enabling NTP server mode...
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

:: Configure NTP announcement flags
echo Configuring announcement flags...
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config /v AnnounceFlags /t REG_DWORD /d 5 /f

:: Restart Windows Time service
echo Restarting Windows Time service...
net stop w32time
net start w32time

:: Add firewall rule for NTP
echo Adding firewall rule for NTP...
netsh advfirewall firewall add rule name="NTP" dir=in action=allow protocol=UDP localport=123

:: Verify configuration
echo Verifying configuration...
w32tm /query /configuration
w32tm /query /status

echo Setup complete! Please review the configuration output above.
pause
```
