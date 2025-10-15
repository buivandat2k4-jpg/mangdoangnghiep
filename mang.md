# Mạng doanh nghiệp vừa và nhỏ 

## Sơ đồ
![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117267619368_ba8ffc022e0e7c24591f5507d7120489.jpg)

## Các chức năng đã thực hiện
1. Các VPC ở các chi nhánh trong sơ đồ nhận được IP DHCP

![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117267188977_3e80c782ab87893061ba397acc4e32eb.jpg)

![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117267381943_6a21d369e431499ce96589b212717a81.jpg)

![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117267491562_9fe5a3e232cbfb3aadd173d70ed14945.jpg)

2. Trên Win Server cài đặt HDCP

![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117268040358_9ddfbf0149bba2c2c5e0f1a89e12c0a5.jpg)

```bash
Tạo các Scope để cấp IP cho các vùng
+ Cấp IP cho trụ sở chính 200 host IP nằm trong dải 172.16.10.0
+ Cấp IP cho chi nhánh 1 50 host IP nằm trong dải 172.16.11.0
+ Cấp IP cho chi nhánh 2 50 host  IP nằm trong dải 172.16.11.64
+ Cấp IP cho chi nhánh 3 50 host  IP nằm trong dải 172.16.11.128
 ```

2. Trên Win Server cài đặt DNS

![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117268280514_cdfb614a5344402e8bd0d3f4d64fed2d.jpg)

```bash
Tạo Forward và Reverse
+ Chuột phải vào tên DNS xong chọn host A -> nhập ip máy server vào -> Create -> add host
+ trên Reverse chuột phải chọn new zone -> next -> xong nhâp ip -> next -> xong finish 
+ Chuột phải chọn New Pointer -> ở host name tìm đến host A đã tạo -> OK
 ```

3. Cài đặt web trên Server và dùng máy win7 truy cập

```bash
+ cài đặt web server, tạo luôn 1 thư mục trong ở C và tạo file web.html trong thư mục.
+ Vào Web Server IIS -> chọn win -> chọn Sites -> xóa web defaul đi -> tạo web mới -> chọn add web -> đặt tên -> ở Physical path chọn đến thư mục web đã tạo ở ổ C 
+ Tao xong vào web vừa tạo -> Chọn default document -> add -> đặt tên là tên file đã tạo web.html
 ```
![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7119507371468_c169d139d329e7fa6e695d1632028191.jpg)

```bash
+ Trên win7 để cùng card mạng và join domain
+ Check ip xem win7 đã nhận ip dhcp từ server chưa 
 ```
![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7117272036386_906d7bd20346fae2844a6fefa0f6a509.jpg)

```bash
+ Nếu đã nhận ip như hình trên thì truy cập web  
 ```
![draw](https://raw.githubusercontent.com/buivandat2k4-jpg/mangdoangnghiep/refs/heads/main/images/z7119521387923_ab9c939711758bf1ef7a2fa1ed2e7e4a.jpg)


### Cấu hình trên các con Router và ISP
 1. R1
 ```bash
 + Cấu hình ip
 hostname R1
 interface FastEthernet0/0
 ip address 2.0.0.2 255.0.0.0
 no sh
 interface FastEthernet1/0
 ip address 1.0.0.2 255.0.0.0
 no sh
 interface FastEthernet4/0
 ip address 172.16.10.1 255.255.255.0
 ip helper-address 172.16.10.5
 no sh
 + cấu hình OSPF
 router ospf 1
 network 1.0.0.0 0.255.255.255 area 0
 network 2.0.0.0 0.255.255.255 area 0
 network 172.16.10.0 0.0.0.255 area 0

 ```
 2. R2
 ```bash
 + Cấu hình ip
 hostname R2
 interface FastEthernet0/0
 ip address 4.0.0.2 255.0.0.0
 no sh
 interface FastEthernet1/0
 ip address 172.16.11.1 255.255.255.192
 ip helper-address 171.16.10.5
 no sh
 interface FastEthernet2/0
 ip address 3.0.0.2 255.0.0.0
 no sh
 + cấu hình OSPF
 router ospf 1
 network 3.0.0.0 0.255.255.255 area 0
 network 4.0.0.0 0.255.255.255 area 0
 network 172.16.11.0 0.0.0.63 area 0
 ```
 3. R3
 ```bash
hostname R3
 interface FastEthernet0/0
 ip address 5.0.0.2 255.0.0.0
 no sh
 interface FastEthernet1/0
 ip address 6.0.0.2 255.0.0.0
 no sh
 interface FastEthernet2/0
 ip address 172.16.11.65 255.255.255.192
 ip helper-address 171.16.10.5
 no sh
 + cấu hình OSPF
 router ospf 1
  network 5.0.0.0 0.255.255.255 area 0
 network 6.0.0.0 0.255.255.255 area 0
 network 172.16.11.64 0.0.0.63 area 0
 ```
 4. R4
 ```bash
 hostname R4
 interface FastEthernet0/0
 ip address 7.0.0.2 255.0.0.0
 no sh
 interface FastEthernet1/0
 ip address 172.16.11.129 255.255.255.192
 ip helper-address 171.16.10.5
 no sh
 interface FastEthernet2/0
 ip address 8.0.0.2 255.0.0.0
 no sh
 + cấu hình OSPF
 router ospf 1
 network 7.0.0.0 0.255.255.255 area 0
 network 8.0.0.0 0.255.255.255 area 0
 network 172.16.11.128 0.0.0.63 area 0
 ```
 5. ISP1
 ```bash
 hostname ISP1
 interface FastEthernet0/0
 ip address dhcp
 no sh
 interface FastEthernet1/0
 ip address 2.0.0.1 255.0.0.0
 no sh
 interface FastEthernet2/0
 ip address 4.0.0.1 255.0.0.0
 no sh
 interface FastEthernet3/0
 ip address 5.0.0.1 255.0.0.0
 no sh
 interface FastEthernet4/0
 ip address 8.0.0.1 255.0.0.0
 no sh
 + cấu hình OSPF
 router ospf 1
  network 2.0.0.0 0.255.255.255 area 0
 network 4.0.0.0 0.255.255.255 area 0
 network 5.0.0.0 0.255.255.255 area 0
 network 8.0.0.0 0.255.255.255 area 0
 network 192.168.111.0 0.0.0.255 area 0
 ```
 6. ISP2
 ```bash
 hostname ISP2
 interface FastEthernet0/0
 ip address dhcp
 no sh
 interface FastEthernet1/0
 ip address 1.0.0.1 255.0.0.0
 no sh
 interface FastEthernet2/0
 ip address 7.0.0.1 255.0.0.0
 no sh
 interface FastEthernet3/0
 ip address 6.0.0.1 255.0.0.0
 no sh
 interface FastEthernet4/0
 ip address 3.0.0.1 255.0.0.0
 no sh
 + cấu hình OSPF
 router ospf 1
 network 1.0.0.0 0.255.255.255 area 0
 network 3.0.0.0 0.255.255.255 area 0
 network 6.0.0.0 0.255.255.255 area 0
 network 7.0.0.0 0.255.255.255 area 0
 network 192.168.111.0 0.0.0.255 area 0
 ```
 