This template can be used to natively monitor Starlink dish status using Zabbix browser monitoring.

<h3>Monitoring configuration</h3>

First, you need to make sure you have Zabbix installed (either a Zabbix proxy or server) on the same network that the Starlink dish and router are on. The next step is to configure Zabbix for browser monitoring.

<h4> WebDriver installation </h4>

```
# podman run --name webdriver -d \
-p 4444:4444 \ 
-p 7900:7900 \
--shm-size="2g" \
--restart=always -d docker.io/selenium/standalone-chrome:latest
Port 4444 will be the port on which the WebDriver will be listening, and port 7900 will be used by NoVNC, which allows us to observe browser behavior in case a browser with a GUI is used.
```
<h4> Zabbix server/proxy configuration </h4>

After WebDriver is installed, we need to set up the communication between Zabbix and the driver. This can be done by editing the Zabbix server/proxy configuration file and updating the following parameters:
```
### Option: WebDriverURL 
# WebDriver interface HTTP[S] URL. For example http://localhost:4444 used with 
# Selenium WebDriver standalone server. 
# 
# WebDriverURL= 
WebDriverURL=http://localhost:4444 
### Option: StartBrowserPollers 
# Number of pre-forked instances of browser item pollers. 
# 
# Range: 0-1000 
# StartBrowserPollers=1 
StartBrowserPollers=5
```
With the configuration parameters in place, restart the Zabbix server/proxy to apply the changes:

systemctl restart zabbix-server
