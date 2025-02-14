# SSL Pinning 

### Prerequisite 
- adb (`sudo apt install adb`)
- frida frida-tools (`pip3 install frida frida-tools`)
- objection (`pip3 install objection`)
- Android Emulator (Android Studio Emulator / GenyMotion / Nox / Ldplayer)

### Steps to Disable SSL Pinning 

1. **Download frida-server according your architecture**\
[Frida-server](https://github.com/frida/frida/releases/tag/16.6.6)

![frida-tools](screenshots/Screenshot%202025-02-15%20001145.png)

![frida-tools](screenshots/Screenshot%202025-02-15%20001154.png)

- Check for architecture using `adb` it's `x86`\
![checking architecture](screenshots/Screenshot%202025-02-15%20001604.png)

1. **Give it proper permission and push to the emulator**
![](screenshots/Screenshot%202025-02-15%20002419.png)

##### Unzip the frida-server
```bash
xz -d frida-server*.xz # unzip the frida server
```
##### Rename it so it's easier to execute while using it's name
```bash
mv frida-server* frida-server
```

##### Give it proper permission
```bash
chmod 777 frida-server
```

##### Push it to the emulator 
```bash
adb root # running adb as root
```
```bash
adb push frida-server /data/local/tmp/
```
#### Run the frida server
```bash
adb shell "/data/local/tmp/frida-server &"
```
 - **leave the terminal open**

Open new terminal and check for connection using `frida-ps -U`
![](screenshots/Screenshot%202025-02-15%20003521.png)
 - **connected**


##### Install Proxy toggle on the emulator
[Proxy Toggle](https://github.com/theappbusiness/android-proxy-toggle)

```bash
adb install -t -r proxy-toggle.apk
adb shell pm grant com.kinandcarta.create.proxytoggle android.permission.WRITE_SECURE_SETTINGS
```

- open proxy toggle and setup host ip and port according to burpsuite
![](screenshots/Screenshot%202025-02-15%20004637.png)

- setup burpsuite proxy listner and install the certificate

**choose all interfaces**

![](screenshots/Screenshot%202025-02-15%20004845.png)

**import the certificate**

![](screenshots/Screenshot%202025-02-15%20004913.png)

**name it accordingly eg. `cert.der` and import**

![](screenshots/Screenshot%202025-02-15%20004943.png)

- Push the certificate to emulator 
![](screenshots/Screenshot%202025-02-15%20005026.png)

##### Renaming the cert in emulator and installing it
![](screenshots/Screenshot%202025-02-15%20005419.png)
![](screenshots/Screenshot%202025-02-15%20005509.png)
![](screenshots/Screenshot%202025-02-15%20005526.png)
![](screenshots/Screenshot%202025-02-15%20005648.png)
- **install for both wifi and vpn and aps**

##### Turn on the proxy 
![](screenshots/Screenshot%202025-02-15%20010056.png)

##### Installing the app I want to test 
```bash
adb install ~/vapt_projects/TruCollect/UAT-9th-SepRelease\ \(1\).apk 
```

### Run Objection to disable ssl pinning
- Open the app on emulator
- List out the packages using previous command `frida-ps -U`
- ![](screenshots/Screenshot%202025-02-15%20004409.png)

**Run objection with the package**
- **make sure frida-server is running**
- **`frida-ps -U` check connection and copy the package name**
- use the following command to enter objection console with selected package for exploration
```bash
objection -g "TruCollect" explore
```
![](screenshots/Screenshot%202025-02-15%20010240.png)

- **After connecting disable sslpinning with**
```bash
android sslpinning disable
```
![](screenshots/Screenshot%202025-02-15%20010536.png)


### TEST The proxy now 
**Got the traffic**
![](screenshots/Screenshot%202025-02-15%20010721.png)
