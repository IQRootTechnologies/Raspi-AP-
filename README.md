# Raspi-AP-
Access Point


    sudo apt-get install -y hostapd dnsmasq
    
    
    sudo nano /etc/hostapd/hostapd.conf
      interface=wlan0
      driver=nl80211
      ctrl_interface=/var/run/hostapd
      ctrl_interface_group=0
      ssid=iqute
      channel=1

      macaddr_acl=0
      auth_algs=1
      ignore_broadcast_ssid=0

      wpa=2
      wpa_passphrase=iqroot@123
      wpa_key_mgmt=WPA-PSK
      wpa_pairwise=TKIP
      rsn_pairwise=CCMP

      ieee80211n=1
      hw_mode=g
    
    
    
    sudo nano /etc/network/interfaces
      iface lo inet loopback
      iface eth0 inet dhcp

      auto wlan0
      allow-hotplug wlan0
          iface wlan0 inet static
		          address 192.168.3.1
		          netmask 255.255.255.0

       up iptables-restore < /etc/iptables.ipv4.nat
       
     sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.bak
     
     sudo nano /etc/dnsmasq.conf
        interface=wlan0
        dhcp-range=192.168.3.5,192.168.3.10,255.255.255.0,12h

     sudo nano /etc/default/hostapd
          DAEMON_CONF="/etc/hostapd/hostapd.conf"
          
     sudo nano /etc/sysctl.conf
          net.ipv4.ip_forward=1 //uncomment 
      
     sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
     sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT  
     sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
  
  
     sudo update-rc.d hostapd enable
     
     sudo reboot




