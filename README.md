# SPCN-011
<p align="center"><a href="https://pve22.ipv9.me/#v1:0:18:4:::::::"> <img src="Screenshots/logo.png"width=300 alt="Paris"></a></p>

# <p align="center">ขั้นตอนในการสร้าง vm and ct on Proxmox cluster บน Proxmox</p>

|<p align="center"> สารบัญ </p>|
| ------------- |
|<DL>[สร้าง master vm (ubuntu-22.04)](#สร้าง-master-vm-ubuntu-2204)<DT>[\|-แปลง vm เป็น template](#แปลง-vm-เป็น-template)<DT><DT>[ \|-clone จาก template เพื่อสร้าง vm](#clone-จาก-template-เพื่อ-สร้าง-vm)</DT></DL>|
|[สร้าง vm จาก os ตัวอื่นๆ](#สร้าง-vm-จาก-os-ตัวอื่นๆ)|
|[สร้าง container template (เลือกจาก CT list)](#สร้าง-container-template-เลือกจาก-ct-list)|
|<DL>[การตั้งค่าต่างๆใน vm ](#การตั้งค่าต่างๆใน-vm)<DT>[\|-การเปิดใช้ qemu guest agent](#การเปิดใช้-qemu-guest-agent)<DT><DT>[ \|-การตั้ง local time update](#การตั้ง-local-time-update)</DT><DT>[\|-การ reset machineid ไม่ให้ ip ชนกัน กับ vm ตัวอื่น](#การ-reset-machineid-ไม่ให้-ip-ชนกัน-กับ-vm-ตัวอื่น)<DT></DL>|


## สร้าง master vm (ubuntu-22.04) 
-------------
[top⬆️](#spcn-011)
1. กดเลือกปุ่ม **Create VM**

<p align="center"> <img src="Screenshots/(39-1).png" ></p>

2. กรอกช่อง **Name** โดยตั้ง [ *ตัวอักษรชื่อภาษาอังกฤษตัวเองอย่างน้อย 3 ตัว-เลข VM ID* ] และช่องDropdown **Resource Pool** ให้เลือก [ *Special_CN* ] แล้วกด **Next**

<p align="center"><img src="Screenshots/(40).png"width=600></p>

3. ช่องDropdown **ISO image** ให้เลือก [ *ubuntu-22.04.1-live-server-amd64* ] แล้วกด **Next** ข้อมูลหลังจากนี้จะเป็นค่า **Default** ทั้งหมดไม่ต้องแก้อะไรทั้งสิ้นจดกว่าจะถึงการตั้งค่าใน VM

<p align="center"><img src="Screenshots/(41).png"width=600></p>

4. ผลที่ได้แถบขวาจะมีชื่อ VM ที่เราตั้งไว้ แล้วในหน้าหลักก็จะแสดงหน้า Status ต่างๆของตัว VM เราในแถบ **Summary** จากนั้นกด **Double Click** ที่ชื่อ VM ของเราในแถบขวา

<p align="center"> <img src="Screenshots/(45).png"width=600></p>

5. ก็จะเข้ามาในระบบ OS ที่เราติดตั้งไว้ทำการ Install ตามขั้นตอน และ นำเข้า SSH มาเชื่อมกับระบบโดยใช้ **Username GitHub** ของตัวเองอย่างเช่นในรูป 

<p align="center"> <img src="Screenshots/(42).png"width=600></p>

6. ผลที่ได้จะเข้ามาในระบบ OS เรียบร้อย 

<p align="center"> <img src="Screenshots/(43).png"width=600></p>

### แปลง vm เป็น template 
[top⬆️](#spcn-011)
1. จากตัว VM ให้เลือก Dropdown **More** มุมขวาบน แล้วเลือก [ *Convert to template* ]

<p align="center"> <img src="Screenshots/(46).png"width=600></p>

2. ผลที่ได้ VM ที่เราสร้างจะกลายเป็นไฟล์ **template** ทันที 

<p align="center"> <img src="Screenshots/(47).png"width=600></p>

### clone จาก template เพื่อ สร้าง vm
[top⬆️](#spcn-011)

1. จากตัว template ให้เลือก Dropdown **More** มุมขวาบน แล้วเลือก [ *Clone* ]

<p align="center"> <img src="Screenshots/(48).png"width=600></p>

2. กรอกช่อง **Name** โดยตั้ง [ *ตัวอักษรชื่อภาษาอังกฤษตัวเองอย่างน้อย 3 ตัว-clone-เลข VM ID* ] และช่องDropdown **Resource Pool** ให้เลือก [ *Special_CN* ] แล้วกด **Clone** สร้างมา 2 ตัว หรือมากกว่านั้นก็ได้

<p align="center"> <img src="Screenshots/(50).png"width=600></p>

3. ผลที่ได้ก็จะได้ VM ตามจำนวนที่เรา clone เลย

<p align="center"> <img src="Screenshots/(53).png"width=600></p>
<p align="center"> <img src="Screenshots/(55).png"width=600></p>

4. ใช้คำสั่งดังนี้เพื่อเปลี่ยนชื่อ Host ของ VM ที่ Clone มา
    ```
    sudo hostnamectl hostname *ชื่ออะไรก็ได้*
    ```
5. ทำการ Reset หรือ Restart os ก็จะเปลี่ยนชื่อ Host ทันที

<p align="center"> <img src="Screenshots/(69).png"width=600></p>

## สร้าง vm จาก os ตัวอื่นๆ 
-------------
[top⬆️](#spcn-011)
1. ทำตามขั้นตอน [การสร้าง master vm ubuntu 2204](#สร้าง-master-vm-ubuntu-2204) แต่ใช้ os ตัวอื่น โดยจะยกตัวอย่างเอา [  *kali-linux-2022-3-live-amd64*  ] และ ตั้งชื่อ VM [ *ตัวอักษรชื่อภาษาอังกฤษตัวเองอย่างน้อย 3 ตัว-otherOS-เลข VM ID* ]

<p align="center"> <img src="Screenshots/(59).png"width=600></p>

2. หลังได้ OS ให้ [เปิดใช้ QEMU Guest Agent และ ตั้ง local time update ของ os](#เปิดใช้-qemu-guest-agent-และ-ตั้ง-local-time-update-ของ-os)

3. ผลที่ได้จะได้ OS ที่มีหน้า UI และ ในหน้า **Summary** จะมีเลข IPs เพิ่มมาหลังจากทำการเปิดใช้  **QEMU Guest Agent**
<p align="center"> <img src="Screenshots/(66).png"width=600></p>
<p align="center"> <img src="Screenshots/(67).png"width=600></p>

## สร้าง container template (เลือกจาก CT list)
-------------
[top⬆️](#spcn-011)
1. กดเลือกปุ่ม **Create CT**

<p align="center"> <img src="Screenshots/(39-2).png"width=600><p>

2. กรอกช่อง **Hostname** [ *ตัวอักษรชื่อภาษาอังกฤษตัวเองอย่างน้อย 3 ตัว-เลข VM ID* ] แล้ว ช่องDropdown **Resource Pool** ให้เลือก [ *Special_CN* ] และกรอกช่อง **Password** ตามที่เรามีอยู่ในระบบ Proxmox

<p align="center"> <img src="Screenshots/(62-1).png"width=600><p>

3. นำเข้า **SSH public key** โดยการกดปุ่ม **Load SSH Key File** จากไฟล์ SSH Key จะอยู่ใน Path [  *user\ชื่อผู้ใช้\.ssh*  ] ([ต้องเปิดใช้ ssh key ก่อนถึงจะมีไฟล์ SSH Key](https://www.youtube.com/watch?v=WgZIv5HI44o&t=65s)) ไฟล์จะชื่อ [ *id_rsa.pub*  ] 

<p align="center"> <img src="Screenshots/(62-2).png"width=600><p>
<p align="center"> <img src="Screenshots/(61).png"width=600><p>

4. กด **Next** มาถึงหน้า **Network** ติ๊ก Radio Button ของ **IPv4** เป็น **DHCP** และ **IPv6** เป็น **SLAAC** แล้วกด **Next** จนติดตั้ง

<p align="center"> <img src="Screenshots/(63).png"width=600><p>

5. ผลที่ได้จะได้ os เหมือนกันแต่ติดตั้งเร็วกว่าปกติ 

<p align="center"> <img src="Screenshots/(64).png"width=600><p>
<p align="center"> <img src="Screenshots/(65).png"width=600><p>

## การตั้งค่าต่างๆใน vm 
-------------
[top⬆️](#spcn-011)

### การเปิดใช้ QEMU Guest Agent

1. **Shutdown** VM แล้วไปแถบ Option ของ VM ตัวนั้น เลือกกด **QEMU Guest Agent**

<p align="center"> <img src="Screenshots/(57).png"width=600></p>

2. เลือกติ๊ก Checkbox **Use QEMU Guest Agent**

<p align="center"> <img src="Screenshots/(56).png"width=600></p>

3. ใช้คำสั่งต่อไปนี้เพื่อติดตั้ง **QEMU Guest Agent**
    ```
    sudo apt-get update 
    sudo apt install qemu-guest-agent
    ```

4. ใช้คำสั่งนี้เพื่อเปิดการทำงาน **QEMU Guest Agent**
    ```
    sudo systemctl start qemu-guest-agent
    ```
    และใช้คำสั่งนี้เพื่อตรวจสอบสถานะการทำงานของ **QEMU Guest Agent**
    ```
    sudo systemctl status qemu-guest-agent
    ```
    <p align="center"> <img src="Screenshots/2022-12-04 011815.jpg"></p>
5. ผลที่ได้ในหน้า **Summary** จะมีเลข IPs เพิ่มมาหลังจากทำการเปิดใช้  **QEMU Guest Agent**

<p align="center"> <img src="Screenshots/(58).png"width=600></p>
<p align="center"> <img src="Screenshots/(68).png"width=600></p>

### การตั้ง local time update
1. เข้ามาใน OS Login แล้วทำการใช้คำสั่งต่อไปนี้เพื่อ **local time update**
    ```
    sudo timedatectl set-timezone Asia/Bangkok
    ```
2. ใช้คำสั่งนี้เพื่อตรวจสอบวันและเวลา

    ```
    date
    ```
    <p align="center"> <img src="Screenshots/2022-12-03 230111.jpg"></p>


### การ Reset MachineID ไม่ให้ IP ชนกัน กับ VM ตัวอื่น

1. เปิดใช้โหมด root โดยคำสั่ง
    ```
    sudo -i
    ```

2. ใช้คำสั่งต่อไปนี้ Reset MachineID ไม่ให้ IP ชนกัน กับ VM ตัวอื่น

    ```
    rm /var/lib/dbus/machine-id
    echo -n > /etc/machine-id
    cat /etc/machine-id
    ln -s /etc/machine-id /var/lib/dbus/machine-id    
    ``` 
    <p align="center"> <img src="Screenshots/(70).png"></p>
3. ทำการ Reset หรือ Restart os 
4. ผลที่ได้ เมื่อนำ VM แต่ละตัวมาทำขั้นตอนข้างต้นจะพบว่า ค่า IPv4 นั้นเปลี่ยนแปลงไป 
 <p align="center"> <img src="Screenshots/(72).png"></p>
 <p align="center"> <img src="Screenshots/(73).png"></p>
ตามรูปนี้ที่เทียบระหว่าง VM clone 1 กับ clone 2 ก่อน Reset MachineID ค่า IP เป็น 172.31.1.147 กับ 172.31.1.28 
<p align="center"> <img src="Screenshots/(74).png"></p>
 <p align="center"> <img src="Screenshots/(75).png"></p>
ตามรูปนี้ที่เทียบระหว่าง VM clone 1 กับ clone 2 หลัง Reset MachineID ค่า IP เป็น 172.31.1.147 กับ 172.31.1.148  ซึ่งการเปลี่ยนนี้ทำให้ IP ของ VM เรียงต่อกันทำให้ไม่เกิดการชนกันของ IP เพราะก่อนทำ Reset MachineID เลข IP จะไม่เรียงต่อกันเหมือนเป็นการสุ่มเลข IP อาจจะมีการซ้ำกันได้เสมอ

------




