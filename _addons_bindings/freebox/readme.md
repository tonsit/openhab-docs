---
id: freebox
label: Freebox
title: Freebox - Bindings
type: binding
description: "This binding integrates the [Freebox Revolution](http://www.free.fr/adsl/freebox-revolution.html) to your openHab installation."
since: 2x
logo: images/addons/freebox.png
install: auto
---

<!-- Attention authors: Do not edit directly. Please add your changes to the appropriate source repository -->

{% include base.html %}

# Freebox Binding

This binding integrates the [Freebox Revolution](http://www.free.fr/adsl/freebox-revolution.html) to your openHab installation.

## Supported Things

This binding supports the following thing types:

| Thing         | Thing Type | Description                                             |
|---------------|------------|---------------------------------------------------------|
| server        | Bridge     | The Freebox Revolution Server.                          |
| phone         | Thing      | The phone wired to the Freebox Revolution.              |
| net_device    | Thing      | A network device on the local network.                  |
| net_interface | Thing      | A network interface from a device on the local network. |
| airplay       | Thing      | An AirPlay device in the local network.                 |

## Discovery

The Freebox Revolution server is discovered automatically through mDNS in the local network. After a Freebox Revolution is discovered and available to openHAB, the binding will automatically discover phone, network devices / interfaces and AirPlay devices with video capability in the local network.
Note that the discovered thing will be setup to use only HTTP API (and not HTTPS) because only an IP can be automatically determined while a domain name is required to use HTTPS with a valid certificate.

## Binding configuration

The binding has the following configuration options, which can be set for "binding:freebox":

| Parameter   | Name         | Description                                                              | Required |
|-------------|--------------|--------------------------------------------------------------------------|----------|
| callbackUrl | Callback URL | URL to use for playing notification sounds, e.g. <http://192.168.0.2:8080> | no       |

## Thing Configuration

### Server

The _server_ bridge thing requires the following configuration parameters:

| Parameter Label         | Parameter ID    | Description                                                                 | Required | Default              |
|-------------------------|-----------------|-----------------------------------------------------------------------------|----------|----------------------|
| Freebox Network Address | fqdn            | The IP address / FQDN of the Freebox Server (can include port number).      | false    | mafreebox.freebox.fr |
| Application token       | appToken        | Token generated by the Freebox Server.                                      | false    |                      |
| Refresh Interval        | refreshInterval | The refresh interval in seconds which is used to poll given Freebox Server. | false    | 30                   |
| Use only HTTP API       | useOnlyHttp     | Use HTTP API even if HTTPS is available.                                    | false    | false                |


If the parameter _ipAddress_ is not set, the binding will use the default address used by Free to access your Freebox Server (mafreebox.freebox.fr).
The bridge thing will initialize only if a valid application token (parameter _appToken_) is filled.

### Phone

The _phone_ thing requires the following configuration parameters:

| Parameter Label              | Parameter ID              | Description                                                                                 | Required | Default |
|------------------------------|---------------------------|---------------------------------------------------------------------------------------------|----------|---------|
| Phone State Refresh Interval | refreshPhoneInterval      | The refresh interval in seconds which is used to poll given Freebox Server for phone state. | false    | 2       |
| Phone Calls Refresh Interval | refreshPhoneCallsInterval | The refresh interval in seconds which is used to poll given Freebox Server for phone calls. | false    | 60      |


### Network device

The _net_device_ thing requires the following configuration parameters:

| Parameter Label | Parameter ID | Description                            | Required |
|-----------------|--------------|----------------------------------------|----------|
| MAC Address     | macAddress   | The MAC address of the network device. | true     |


### Network interface

The _net_interface_ thing requires the following configuration parameters:

| Parameter Label | Parameter ID | Description                                         | Required |
|-----------------|--------------|-----------------------------------------------------|----------|
| IP Address      | ipAddress    | The IP address (v4 or v6) of the network interface. | true     |


### AirPlay device

The _airplay_ thing requires the following configuration parameters:

| Parameter Label | Parameter ID | Description                 | Required |
|-----------------|--------------|-----------------------------|----------|
| Name            | name         | Name of the AirPlay device  | true     |
| Password        | password     | AirPlay password            | false    |
| Accept all MP3  | acceptAllMp3 | Accept any bitrate for MP3 audio or only bitrates greater than 64 kbps | false    |

## HTTPS Access

Each Freebox server is now automatically assigned a random domain name (in addition to mafreebox.freebox.fr that can be used inside the local network), and an associated TLS certificate to enable secure access to API.
This certificate is also valid for the domain name mafreebox.freebox.fr too.
You must validate the certificate chain, by using the following Freebox ECC Root CA and Freebox RSA Root CA:

```
-----BEGIN CERTIFICATE-----
MIICWTCCAd+gAwIBAgIJAMaRcLnIgyukMAoGCCqGSM49BAMCMGExCzAJBgNVBAYT
AkZSMQ8wDQYDVQQIDAZGcmFuY2UxDjAMBgNVBAcMBVBhcmlzMRMwEQYDVQQKDApG
cmVlYm94IFNBMRwwGgYDVQQDDBNGcmVlYm94IEVDQyBSb290IENBMB4XDTE1MDkw
MTE4MDIwN1oXDTM1MDgyNzE4MDIwN1owYTELMAkGA1UEBhMCRlIxDzANBgNVBAgM
BkZyYW5jZTEOMAwGA1UEBwwFUGFyaXMxEzARBgNVBAoMCkZyZWVib3ggU0ExHDAa
BgNVBAMME0ZyZWVib3ggRUNDIFJvb3QgQ0EwdjAQBgcqhkjOPQIBBgUrgQQAIgNi
AASCjD6ZKn5ko6cU5Vxh8GA1KqRi6p2GQzndxHtuUmwY8RvBbhZ0GIL7bQ4f08ae
JOv0ycWjEW0fyOnAw6AYdsN6y1eNvH2DVfoXQyGoCSvXQNAUxla+sJuLGICRYiZz
mnijYzBhMB0GA1UdDgQWBBTIB3c2GlbV6EIh2ErEMJvFxMz/QTAfBgNVHSMEGDAW
gBTIB3c2GlbV6EIh2ErEMJvFxMz/QTAPBgNVHRMBAf8EBTADAQH/MA4GA1UdDwEB
/wQEAwIBhjAKBggqhkjOPQQDAgNoADBlAjA8tzEMRVX8vrFuOGDhvZr7OSJjbBr8
gl2I70LeVNGEXZsAThUkqj5Rg9bV8xw3aSMCMQCDjB5CgsLH8EdZmiksdBRRKM2r
vxo6c0dSSNrr7dDN+m2/dRvgoIpGL2GauOGqDFY=
-----END CERTIFICATE-----
```

```
-----BEGIN CERTIFICATE-----
MIIFmjCCA4KgAwIBAgIJAKLyz15lYOrYMA0GCSqGSIb3DQEBCwUAMFoxCzAJBgNV
BAYTAkZSMQ8wDQYDVQQIDAZGcmFuY2UxDjAMBgNVBAcMBVBhcmlzMRAwDgYDVQQK
DAdGcmVlYm94MRgwFgYDVQQDDA9GcmVlYm94IFJvb3QgQ0EwHhcNMTUwNzMwMTUw
OTIwWhcNMzUwNzI1MTUwOTIwWjBaMQswCQYDVQQGEwJGUjEPMA0GA1UECAwGRnJh
bmNlMQ4wDAYDVQQHDAVQYXJpczEQMA4GA1UECgwHRnJlZWJveDEYMBYGA1UEAwwP
RnJlZWJveCBSb290IENBMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA
xqYIvq8538SH6BJ99jDlOPoyDBrlwKEp879oYplicTC2/p0X66R/ft0en1uSQadC
sL/JTyfgyJAgI1Dq2Y5EYVT/7G6GBtVH6Bxa713mM+I/v0JlTGFalgMqamMuIRDQ
tdyvqEIs8DcfGB/1l2A8UhKOFbHQsMcigxOe9ZodMhtVNn0mUyG+9Zgu1e/YMhsS
iG4Kqap6TGtk80yruS1mMWVSgLOq9F5BGD4rlNlWLo0C3R10mFCpqvsFU+g4kYoA
dTxaIpi1pgng3CGLE0FXgwstJz8RBaZObYEslEYKDzmer5zrU1pVHiwkjsgwbnuy
WtM1Xry3Jxc7N/i1rxFmN/4l/Tcb1F7x4yVZmrzbQVptKSmyTEvPvpzqzdxVWuYi
qIFSe/njl8dX9v5hjbMo4CeLuXIRE4nSq2A7GBm4j9Zb6/l2WIBpnCKtwUVlroKw
NBgB6zHg5WI9nWGuy3ozpP4zyxqXhaTgrQcDDIG/SQS1GOXKGdkCcSa+VkJ0jTf5
od7PxBn9/TuN0yYdgQK3YDjD9F9+CLp8QZK1bnPdVGywPfL1iztngF9J6JohTyL/
VMvpWfS/X6R4Y3p8/eSio4BNuPvm9r0xp6IMpW92V8SYL0N6TQQxzZYgkLV7TbQI
Hw6v64yMbbF0YS9VjS0sFpZcFERVQiodRu7nYNC1jy8CAwEAAaNjMGEwHQYDVR0O
BBYEFD2erMkECujilR0BuER09FdsYIebMB8GA1UdIwQYMBaAFD2erMkECujilR0B
uER09FdsYIebMA8GA1UdEwEB/wQFMAMBAf8wDgYDVR0PAQH/BAQDAgGGMA0GCSqG
SIb3DQEBCwUAA4ICAQAZ2Nx8mWIWckNY8X2t/ymmCbcKxGw8Hn3BfTDcUWQ7GLRf
MGzTqxGSLBQ5tENaclbtTpNrqPv2k6LY0VjfrKoTSS8JfXkm6+FUtyXpsGK8MrLL
hZ/YdADTfbbWOjjD0VaPUoglvo2N4n7rOuRxVYIij11fL/wl3OUZ7GHLgL3qXSz0
+RGW+1oZo8HQ7pb6RwLfv42Gf+2gyNBckM7VVh9R19UkLCsHFqhFBbUmqwJgNA2/
3twgV6Y26qlyHXXODUfV3arLCwFoNB+IIrde1E/JoOry9oKvF8DZTo/Qm6o2KsdZ
dxs/YcIUsCvKX8WCKtH6la/kFCUcXIb8f1u+Y4pjj3PBmKI/1+Rs9GqB0kt1otyx
Q6bqxqBSgsrkuhCfRxwjbfBgmXjIZ/a4muY5uMI0gbl9zbMFEJHDojhH6TUB5qd0
JJlI61gldaT5Ci1aLbvVcJtdeGhElf7pOE9JrXINpP3NOJJaUSueAvxyj/WWoo0v
4KO7njox8F6jCHALNDLdTsX0FTGmUZ/s/QfJry3VNwyjCyWDy1ra4KWoqt6U7SzM
d5jENIZChM8TnDXJzqc+mu00cI3icn9bV9flYCXLTIsprB21wVSMh0XeBGylKxeB
S27oDfFq04XSox7JM9HdTt2hLK96x1T7FpFrBTnALzb7vHv9MhXqAT90fPR/8A==
-----END CERTIFICATE-----
```

First copy and paste on your server running openHAB these two public certificates in 2 files named for example /freeboxECC.crt and /freeboxRSA.crt.
Then you have to import these two certificate as trusted public certificate for your installed Java Runtime Environment.
On Linux server, the command is:

```
sudo keytool -import -trustcacerts -file /freeboxECC.crt -alias Freebox -keystore $JAVA_HOME/jre/lib/security/cacerts
sudo keytool -import -trustcacerts -file /freeboxRSA.crt -alias Freebox -keystore $JAVA_HOME/jre/lib/security/cacerts
sudo rm /freeboxECC.crt /freeboxRSA.crt
```
## Authentication

You'll have to authorize openHAB to connect to your Freebox. Here is the process described :

**Step 1** At binding startup, if no token is recorded in the Freebox Server (bridge) configuration, the following message will be displayed in the OSGi console :

```
            ####################################################################
            # Please accept activation request directly on your freebox        #
            # Once done, record Apptoken in the Freebox Item configuration     #
            # bEK7a7O8GkxxxxxxxxxxXBsKu/xxxttttwj5bXSssd5gUvSXs4vrpuhZwelEo804 #
            ####################################################################
```

**Step 2** Run to your Freebox and approve the pairing request for openHAB Freebox Binding that is displayed on the Freebox screen

**Step 3** Record the apptoken in the Freebox Server (bridge) configuration

**Optionally** you can log in your Freebox admin console to allocate needed rights to openhab

Once initialized, the thing will generate all available channels.

## Channels

The following channels are supported:

| Thing         | Channel Type ID          | Item Type | Access Mode | Description                                                                     |
|---------------|--------------------------|-----------|-------------|---------------------------------------------------------------------------------|
| server        | fwversion                | String    | R           | Version of the Freebox Server firmware                                          |
| server        | uptime                   | Number    | R           | Time since last reboot of the Freebox Server                                    |
| server        | restarted                | Switch    | R           | Indicates whether the Freebox server hase restarted during the last poll period |
| server        | tempcpum                 | Number    | R           | Actual measured CPU Marvell temperature                                         |
| server        | tempcpub                 | Number    | R           | Actual measured CPU Broadcom (xDSL) temperature                                 |
| server        | tempswitch               | Number    | R           | Actual measured switch temperature                                              |
| server        | fanspeed                 | Number    | R           | Actual measured fan speed (rpm)                                                 |
| server        | reboot                   | Switch    | W           | Reboots the Freebox server                                                      |
| server        | lcd_brightness           | Number    | RW          | Brightness level of the screen in percent                                       |
| server        | lcd_orientation          | Number    | RW          | Screen Orientation in degrees (0 or 90 or 180 or 270)                           |
| server        | lcd_forced               | Switch    | RW          | Indicates whether the screen orientation forced                                 |
| server        | wifi_status              | Switch    | RW          | Indicates whether the WiFi network is enabled                                   |
| server        | ftp_status               | Switch    | RW          | Indicates whether the FTP server  is enabled                                    |
| server        | airmedia_status          | Switch    | RW          | Indicates whether Air Media  is enabled                                         |
| server        | upnpav_status            | Switch    | RW          | Indicates whether UPnP AV  is enabled                                           |
| server        | sambafileshare_status    | Switch    | RW          | Indicates whether Window File Sharing  is enabled                               |
| server        | sambaprintershare_status | Switch    | RW          | Indicates whether Window Printer Sharing  is enabled                            |
| server        | xdsl_status              | String    | R           | Status of the xDSL line                                                         |
| server        | line_status              | String    | R           | Status of network line connexion                                                |
| server        | ipv4                     | String    | R           | Public IP Address of the Freebox Server                                         |
| server        | rate_up                  | Number    | R           | Current upload rate in byte/s                                                   |
| server        | rate_down                | Number    | R           | Current download  rate in byte/s                                                |
| server        | bytes_up                 | Number    | R           | Total uploaded bytes since last connection                                      |
| server        | bytes_down               | Number    | R           | Total downloaded  bytes since last connection                                   |
| phone         | state#onhook             | Switch    | R           | Indicates whether the phone is on hook                                          |
| phone         | state#ringing            | Switch    | R           | Is the phone ringing                                                            |
| phone         | any#call_number          | String    | R           | Last call: number                                                               |
| phone         | any#call_duration        | Number    | R           | Last call: duration in seconds                                                  |
| phone         | any#call_timestamp       | DateTime  | R           | Last call: creation timestamp                                                   |
| phone         | any#call_status          | String    | R           | Last call: type (ingoing, outgoing, missed)                                     |
| phone         | any#call_name            | String    | R           | Last call: called name for outgoing calls. Caller name for incoming calls       |
| phone         | accepted#call_number     | String    | R           | Last accepted call: number                                                      |
| phone         | accepted#call_duration   | Number    | R           | Last accepted call: duration in seconds                                         |
| phone         | accepted#call_timestamp  | DateTime  | R           | Last accepted call: creation timestamp                                          |
| phone         | accepted#call_name       | String    | R           | Last accepted call: caller name                                                 |
| phone         | missed#call_number       | String    | R           | Last missed call: number                                                        |
| phone         | missed#call_duration     | Number    | R           | Last missed call: duration in seconds                                           |
| phone         | missed#call_timestamp    | DateTime  | R           | Last missed call: creation timestamp                                            |
| phone         | missed#call_name         | String    | R           | Last missed call: caller name                                                   |
| phone         | outgoing#call_number     | String    | R           | Last outgoing call: number                                                      |
| phone         | outgoing#call_duration   | Number    | R           | Last outgoing call: duration in seconds                                         |
| phone         | outgoing#call_timestamp  | DateTime  | R           | Last outgoing call: creation timestamp                                          |
| phone         | outgoing#call_name       | String    | R           | Last outgoing call: called name                                                 |
| net_device    | reachable                | Switch    | R           | Indicates whether the network device is reachable                               |
| net_interface | reachable                | Switch    | R           | Indicates whether the network interface is reachable                            |
| airplay       | playurl                  | String    | W           | Play an audio or video media from the given URL                                 |
| airplay       | stop                     | Switch    | W           | Stop the media playback                                                         |

**Example**

### Things

Here is an example with minimal configuration parameters (using default values).
It will first connect to mafreebox.freebox.fr using HTTPS (and will failback to HTTP if HTTPS access is not available).

```
Bridge freebox:server:fb "Freebox Revolution" [ appToken="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" ] {
    Thing phone Phone "Phone"
    Thing net_device tv1 "TV living room" [ macAddress="XX:XX:XX:XX:XX:XX" ]
    Thing net_interface tv2 "TV bedroom" [ ipAddress="192.168.0.100" ]
    Thing airplay player "Freebox Player (AirPlay)" [ name="Freebox Player" ]
}
```

Here is another example overwritting default configuration parameters:

```
Bridge freebox:server:fb "Freebox Revolution" [ fqdn="abcdefgh.fbxos.fr", appToken="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", refreshInterval=20, useOnlyHttp=false ] {
    Thing phone Phone "Phone" [ refreshPhoneInterval=10, refreshPhoneCallsInterval=120 ]
    Thing net_device tv1 "TV living room" [ macAddress="XX:XX:XX:XX:XX:XX" ]
    Thing net_interface tv2 "TV bedroom" [ ipAddress="192.168.0.100" ]
    Thing airplay player "Freebox Player (AirPlay)" [ name="Freebox Player", password="1111", acceptAllMp3=false ]
}
```

### Items

```
String Freebox_xdsl_status "Freebox state [%s]" {channel="freebox:server:fb:xdsl_status"}
String Freebox_cs_state "State [%s]" {channel="freebox:server:fb:line_status"}
String Freebox_cs_ipv4 "ipV4 [%s]" {channel="freebox:server:fb:ipv4"}
Number Freebox_cs_rate_up "Upload rate [%d b/s]" {channel="freebox:server:fb:rate_up"}
Number Freebox_cs_rate_down "Download rate [%d b/s]" {channel="freebox:server:fb:rate_down"}
Number Freebox_cs_bytes_up "Uploaded [%d bytes]" {channel="freebox:server:fb:bytes_up"}
Number Freebox_cs_bytes_down "Downloaded [%d bytes]" {channel="freebox:server:fb:bytes_down"}
String Freebox_sys_firmware_version "Version [%s]" {channel="freebox:server:fb:fwversion"}
Switch Freebox_reboot "Reboot freebox" {channel="freebox:server:fb:reboot"}
Switch Freebox_restarted "Restarted" {channel="freebox:server:fb:restarted"}
Number Freebox_sys_uptime "Uptime [%d sec]" {channel="freebox:server:fb:uptime"}
Number Freebox_sys_temp_cpum "Temp cpum [%d Â°C]" <temperature> {channel="freebox:server:fb:tempcpum"}
Number Freebox_sys_temp_cpub "Temp cpub [%d Â°C]" <temperature> {channel="freebox:server:fb:tempcpub"}
Number Freebox_sys_temp_sw "Temp sw [%d Â°C]" <temperature> {channel="freebox:server:fb:tempswitch"}
Number Freebox_sys_fan_rpm "Fan [%d rpm]" <fan> {channel="freebox:server:fb:fanspeed"}
Switch FreeboWifi "Wifi" {autoupdate="false", channel="freebox:server:fb:wifi_status"}
Switch FreeboxFTP "FTP" {autoupdate="false", channel="freebox:server:fb:ftp_status"}
Switch FreeboxUPnPAV "UPnP AV" {autoupdate="false", channel="freebox:server:fb:upnpav_status"}
Switch FreeboxAirMedia "AirMedia" {autoupdate="false", channel="freebox:server:fb:airmedia_status"}
Switch FreeboxSambaFiles "Win File Share" {autoupdate="false", channel="freebox:server:fb:sambafileshare_status"}
Switch FreeboxSambaPrinters "Win Print Share" {autoupdate="false", channel="freebox:server:fb:sambaprintershare_status"}
Number Freebox_lcd_brightness "Brightness [%d %%]" {channel="freebox:server:fb:lcd_brightness"}
Number Freebox_lcd_orientation "Orientation [%d Â°]" {channel="freebox:server:fb:lcd_orientation"}
Switch Freebox_lcd_forced "LCD Forced" {channel="freebox:server:fb:lcd_forced"}

Switch Freebox_onhook "Phone on hook" {channel="freebox:phone:fb:Phone:state#onhook"}
Switch Freebox_ringing "Phone ringing" {channel="freebox:phone:fb:Phone:state#ringing"}
String Freebox_call_number "Last call [%s]" {channel="freebox:phone:fb:Phone:any#call_number"}
String Freebox_call_name "Name [%s]" {channel="freebox:phone:fb:Phone:any#call_name"}
Number Freebox_call_duration "Duration [%d sec]" {channel="freebox:phone:fb:Phone:any#call_duration"}
DateTime Freebox_call_ts "TimeStamp [%1$tA %1$td %1$tR]" <calendar> {channel="freebox:phone:fb:Phone:any#call_timestamp"}
String Freebox_call_status "Status [%s]" {channel="freebox:phone:fb:Phone:any#call_status"}
String Freebox_accepted_call_number "Last accepted call [%s]" {channel="freebox:phone:fb:Phone:accepted#call_number"}
String Freebox_accepted_call_name "Caller name [%s]" {channel="freebox:phone:fb:Phone:accepted#call_name"}
Number Freebox_accepted_call_duration "Duration [%d sec]" {channel="freebox:phone:fb:Phone:accepted#call_duration"}
DateTime Freebox_accepted_call_ts "TimeStamp [%1$tA %1$td %1$tR]" <calendar> {channel="freebox:phone:fb:Phone:accepted#call_timestamp"}
String Freebox_missed_call_lastnum "Last missed call [%s]" {channel="freebox:phone:fb:Phone:missed#call_number"}
String Freebox_missed_call_name "Caller name [%s]" {channel="freebox:phone:fb:Phone:missed#call_name"}
Number Freebox_missed_call_duration "Duration [%d sec]" {channel="freebox:phone:fb:Phone:missed#call_duration"}
DateTime Freebox_missed_call_ts "TimeStamp [%1$tA %1$td %1$tR]" <calendar> {hannel="freebox:phone:fb:Phone:missed#call_timestamp"}
String Freebox_outcall_lastnum "Last outgoing call [%s]" {channel="freebox:phone:fb:Phone:outgoing#call_number"}
String Freebox_outcall_name "Called name [%s]" {channel="freebox:phone:fb:Phone:outgoing#call_name"}
Number Freebox_outcall_duration "Duration [%d sec]" {channel="freebox:phone:fb:Phone:outgoing#call_duration"}
DateTime Freebox_outcall_ts "TimeStamp [%1$tA %1$td %1$tR]" <calendar> {channel="freebox:phone:fb:Phone:outgoing#call_timestamp"}

Switch TVLivingRoom "TV living room" <television> {channel="freebox:net_device:fb:tv1:reachable"}
Switch TVBedroom "TV bedroom" <television> {channel="freebox:net_interface:fb:tv2:reachable"}

String freebox_player_playurl "URL [%s]" { channel="freebox:airplay:fb:player:playurl" }
Switch freebox_player_stop "Stop playback" { channel="freebox:airplay:fb:player:stop" }
```