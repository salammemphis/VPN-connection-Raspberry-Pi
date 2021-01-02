# VPN-connection-Raspberry-Pi

Configure Raspberry Pi to connect to Personal or Enterprise Network

1. Add a configuration file in following location 
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
2. Add following content in that file based on WIFI type

    Personal authentication (WPA, WPA2)

    network={
    ssid="YOUR_NETWORK_NAME"
    proto=RSN
    key_mgmt=WPA-PSK
    pairwise=CCMP TKIP
    group=CCMP TKIP
    psk="YOUR_NETWORK_PASSWORD"
    }

    Enterprise authentication (MSCHAPV2)

    network={
    ssid="YOUR_NETWORK_NAME"
    proto=RSN
    key_mgmt=WPA-EAP
    pairwise=CCMP TKIP
    group=CCMP TKIP
    identity="YOUR_USER_NAME"
    password=hash:YOUR_PASSWORD_HASH
    phase1="peaplabel=0"
    phase2="auth=MSCHAPV2"
    }

3. Generate and replace Hash password in above file 
    #Personal authentication (WPA, WPA2)
    wpa_passphrase YOUR_NETWORK_NAME YOUR_PASSWORD

    Enterprise authentication (MSCHAPV2)
    echo -n 'YOUR_PASSWORD' | iconv -t utf16le | openssl md4
  
 4. Modify following file 
    /etc/network/interface

5. add following content in wlan0 section of that file
    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet dhcp
      pre-up wpa_supplicant -B -Dwext -i wlan0 -c/etc/wpa_supplicant/wpa_supplicant.conf
      post-down killall -q wpa_supplicant
