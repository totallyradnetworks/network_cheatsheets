# Palo Alto PA-200 Configuration Step-by-Step

1. Console into the firewall
   1. Default Creds - admin/admin
   2. enter config mode
   ```
   configure
   ```
   3. see what options you have available
   ```
   set ?
   ```
   4. set device to a static ip config
   ```
   set deviceconfig systemtype static
   ```
   5. set management interface ip
   ```
   set deviceconfig system ip-address 172.20.1.100 netmask 255.255.255.0
   ```
   6. set default gateway
   ```
   set deviceconfig system default-gateway 172.20.1.1
   ```
   7. commit the settings
   ```
   commit
   ```
   8. navigate to the web gui using the mgmt interface ip