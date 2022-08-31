- [Overview](#overview)
  * [Mô hình hoạt động](#mô-hình-hoạt-động)
  * [Install](#install)
  * [Individual Config](#individual-config)
- [Commands](#commands)
- [Inventory](#inventory)
- [Playbook](#playbook)
- [Module](#module)
  * [Nhóm Module](#nhóm-module)
  * [Modules details](#modules-details)
- [Conditionals and Register](#conditionals-and-register)
- [Loops](#loops)
- [Roles](#roles)
  * [Definition](#definition)
  * [Create roles](#create-roles)
- [Ansible Galaxy](#ansible-galaxy)
- [Terms](#terms)

# Overview

- Ansible là một công cụ "Configuration Management" hỗ trợ lên lịch, cấu hình, cài đặt và quản lý hệ thống một cách tự động (automation và orchestration). Các công cụ này giúp đơn giản, tự động hóa thực hiện triển khai hệ thống, hạn chế những công việc lặp đi lặp lại tiết kiệm thời gian.



![17d8b19782744b66bbb61017cf94a420srSJdioGtWXDie43-9](https://user-images.githubusercontent.com/43572616/181192343-d172ff8e-2e69-4243-91df-51cf58cfa55e.png)


___
**Ứng dụng / Khi nào sử dụng Ansible:**



![17d8b19782744b66bbb61017cf94a420srSJdioGtWXDie43-12](https://user-images.githubusercontent.com/43572616/181192427-775d4732-6e2a-440b-994b-343b969d8139.png)





- Configuration Management: Khi doanh nghiệp hay cá nhân sử dụng nhiều thiết bị hay server, cần cài đặt, cấu hình môi trường trên từng thiết bị. Ansible giúp quản lý cấu hình tập trung các dịch vụ, không cần phải tốn công chỉnh sửa cấu hình trên từng server.
 
- Application Deployment: Deploy ứng dụng hàng loạt, giúp quản lý hiệu quả vòng đời của ứng dụng từ giai đoạn dev cho đến production.
 
- Security & Compliance: Quản lý các chính sách về an toàn thông tin một cách đồng bộ trên nhiều sản phẩm và môi trường khác nhau như deploy policy hay cấu hình firewall hàng loạt trên nhiều server,…
 
- Tự động hóa việc cập nhật, bảo trì nhiều hệ thống hay triển khai một ứng dụng từ xa
 
- Provisioning: Khởi tạo VM, container hàng loạt trên cloud dựa trên API - OpenStack, AWS, Google Cloud, Azure…


___
**Ưu điểm Ansible:**
  - Clear: Ansible sử dụng cú pháp đơn giản (YAML) và dễ hiểu

![1](https://user-images.githubusercontent.com/43572616/181192780-944e2abe-b72f-4a37-994b-d1236ef6ba6d.png)

![17d8b19782744b66bbb61017cf94a420srSJdioGtWXDie43-10](https://user-images.githubusercontent.com/43572616/181192841-64f9c967-ffb3-47bb-9dc4-0e6ec3e35fe5.png)



- Fast: cài đặt nhanh chóng và không phải cài đặt phần mềm hay daemon nào khác trên server 



- Complete: phương pháp tiếp cận batteries included giúp có mọi thứ cần trong một package hoàn chỉnh



- Efficient: Ansible sử dụng kiến trúc agentless không cần cài đặt phần mềm lên các agent, chỉ cần cài đặt tại master. Việc không có phần mềm bổ sung trên các agent sẽ giúp tiết kiệm tài nguyên hơn.



- Secure: Ansible sử dụng SSH và không yêu cầu thêm mở port hoặc daemon nên tránh bị truy cập vào máy chủ qua port hay daemon.



- Ansible nhẹ, độc lập không có bất kỳ ràng buộc nào liên quan đến hệ điều hành hay phần cứng cơ bản nào
 
- Ansible có thể giao tiếp với rất nhiều OS, platform và loại thiết bị khác nhau từ Ubuntu, VMware, CentOS, Windows cho tới Azure, AWS, các thiết bị mạng Cisco và Juniper,… mà hoàn toàn không cần agent khi giao tiếp.


## Mô hình hoạt động

![8335ed0c-7cfe-41b6-b7cf-bd37e05979a7](https://user-images.githubusercontent.com/43572616/181193375-fad5a232-3995-4a27-bd00-676ba059fe28.png)


- **Management Node** là Ansible Server là nơi quản lý các nodes điều khiển toàn bộ quá trình thực thi của playbook
 
- **Playbook** sẽ chứa chi tiết tất cả những gì muốn thực hiện với các server mà admin muốn quản lý và cách thức thực hiện chúng.  
  - Mỗi một thao tác trong Playbook gọi là một Task (Cài đặt, khởi động, dừng,....)
  - Sử dụng Module để tạo thành Task (Ví dụ: muốn cài đặt một gói trên CentOs7 ta sử dụng Module yum của Ansible)
 
- Các tệp **Inventory** cung cấp danh sách các máy chủ mà các module Ansible cần để chạy
 
- Sau khi đọc được các host mà cần chạy ở Inventory thì Management Node sẽ thực hiện việc connect tới các host này thông qua SSH connection và thực thi các modules:



<img width="617" alt="ansible-overview" src="https://user-images.githubusercontent.com/43572616/181193403-545662e2-f86a-4861-bed6-d8f4c157ec68.png">


## Install

- **Link: <https://docs.ansible.com/ansible/latest/installation_guide/index.html>**

___
**Tạo 3 máy ảo:**

Nếu đổi hostname thì phải chỉnh /etc/hostname và /etc/hosts

![image](https://user-images.githubusercontent.com/43572616/181193725-e6d0a0a8-2219-4ff5-bcb0-1594cd31020a.png)

![image](https://user-images.githubusercontent.com/43572616/181193740-27e766ac-3f47-4f25-afaa-d03153e6279f.png)

![image](https://user-images.githubusercontent.com/43572616/181193771-bed17ac3-f082-4ebd-bbc7-20c709d9893d.png)

![image](https://user-images.githubusercontent.com/43572616/181193795-d2829e46-7f0f-42d8-95d3-491af1956729.png)


|ansible\_controller|192.168.23.142|
| :- | :- |
|ansible\_target\_1|192.168.23.143|
|ansible\_target\_2|192.168.23.144|

___
**Yum setup:**

`yum install epel-release -y`

`yum install ansible -y`

![image](https://user-images.githubusercontent.com/43572616/181194290-20a5a191-264c-4421-b1ac-d6e21e21f30a.png)

___
**Trên ansible-controller, tự tạo inventory file:**

`mkdir test\_proj`

`cd test\_proj/`

`cat > inventory.txt`

**Nội dung:** 

|<p>target1 ansible\_host=192.168.23.143 ansible\_password=12345</p><p>target2 ansible\_host=192.168.23.144 ansible\_password=12345</p>|
| :-: |

![image](https://user-images.githubusercontent.com/43572616/181194374-3a33cdb7-cad0-4d5f-82d4-cd9fb655c472.png)



- **Ping:**

`ansible target1 -m ping -i inventory.txt`

![image](https://user-images.githubusercontent.com/43572616/181194745-c0191741-be93-4629-bbfc-1743482ecfd3.png)



`ansible target2 -m ping -i inventory.txt`

![image](https://user-images.githubusercontent.com/43572616/181194776-09448a83-6e13-4c37-9b8b-736634c4f349.png)

___
** Nếu có thông báo lỗi:**

```
target2 | FAILED! => {
    "msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host."
}
```


- **Nếu gặp lỗi này có 2 cách:**



***Cách 1:*** ssh đến máy agent, rồi ping bình thường

![image](https://user-images.githubusercontent.com/43572616/181194889-7723c27d-8f74-45cb-95ba-3b7eea8d6f4f.png)
![image](https://user-images.githubusercontent.com/43572616/181194912-423398e8-120c-49d4-a8a8-a517a4bc351b.png)



***Cách 2:*** chỉnh file config của ansible:

`vi /etc/ansible/ansible.cfg`

![image](https://user-images.githubusercontent.com/43572616/181194980-4232a5a5-13a5-4a1d-803f-898f2b661950.png)

Bỏ cmt của #host\_key\_checking = False:

![image](https://user-images.githubusercontent.com/43572616/181195007-11c60242-a184-429e-8226-f0e2c8f91c94.png)

Ping bình thường:

![image](https://user-images.githubusercontent.com/43572616/181195063-7b906882-a2bc-4efb-84b2-e1d32cdf2770.png)


## Individual Config

|ansible\_controller|192.168.23.142|
| :- | :- |
|ansible\_target\_1|192.168.23.143|
|ansible\_target\_2|192.168.23.144|


**Tạo tài khoản truy cập SSH trên máy agent (option)**

- Do policy của hệ thống giới hạn truy cập tài khoản root cũng như để thuận tiện cho việc quản lý (tránh dùng chung account của người quản trị) thì nên tạo riêng 1 tài khoản khác phục vụ cho ansible ở trên máy agent

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`useradd -c "dat's ansible" dat\_ansible`

&emsp;&emsp;Để đổi mật khẩu tài khoản vừa tạo:*

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `passwd dat\_ansible`


&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181199566-a004a746-5333-4aaf-bb1d-0fb0805ed20a.png)



- Cấu hình sudo cho phép tài khoản ansible sử dụng không cần password:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`visudo`

&emsp;&emsp;&emsp;Thêm các dòng:

```
dat_ansible     ALL=(ALL)       ALL
%dat_ansible    ALL=(ALL)       NOPASSWD: ALL
```

***

**Tạo SSH key trên máy master**

- Để thuận tiện cho việc sử dụng Ansible cũng như giới hạn 1 số hệ thống chỉ cho phép xác thực qua key. (Ansible có hỗ trợ kết nối thông qua password)



- **Tạo SSH key:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ssh-keygen -C "ansible@master"`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181199809-04b053e3-2fef-4b10-970b-0744a6b3c92f.png)

&emsp;&emsp;&emsp;Nhập nơi lưu key:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`/home/datnt/.ansible/ansible\_key`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181199893-cc972a15-0074-4046-986b-de9874035866.png)



- 2 key public và private được tạo:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181199933-97eea2a4-3d40-4ca0-ad71-2a305c3adbb2.png)

***

**Copy SSH key sang máy agent**

- **Copy Public key sang tài khoản dat\_ansible của máy agent**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ssh-copy-id -i .ansible/ansible\_key.pub dat\_ansible@192.168.23.143`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181199997-e0be4179-d135-4802-8665-54a449a08ca9.png)



- **Hoặc có thể chuyến sang root**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ssh-copy-id -i ansible\_key.pub root@192.168.23.143`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181200058-af806fc5-0770-4c85-8d0d-4994ffdc1c45.png)



- **SSH thử:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ssh -i .ansible/ansible\_key dat\_ansible@192.168.23.143`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181200091-876ae37d-5ba7-436c-956a-8d7631b05f26.png)

***
**Cấu hình ansible**

- **Tạo file inventory**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`touch .ansible/hosts`



- **Chỉnh sửa file inventory:**

```
	[webservers]
	web1 ansible_ssh_host=192.168.23.143 ansible_ssh_private_key_file=.ansible/ansible_key ansible_ssh_user=dat_ansible
```


- **Ping:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ansible web1 -m ping -i .ansible/hosts`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181200225-93e12673-9f6b-4ee3-8003-e6af5ab82522.png)

***
**Chạy thử playbook**

- **Tạo playbook.yml tại .ansible:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`vi playbook.yml`



- **Thêm nội dung:**

```
	-
	        name: lab
	        hosts:  web1
	        tasks:
	                - name: docker hello-world
	                  command: sudo docker run hello-world
```


- **Run playbook:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ansible-playbook -i hosts playbook.yml`

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181200389-1a25107c-72f1-4df7-9791-fdb2132a0be6.png)

***
***
# Commands


**Cấu trúc chung:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ansible [tên host] -m [tên module] -a [tham số truyền vào module]`

- -m là loại module
- -i : inventory host. Trỏ thư viện group\_host cần gọi, mặc định nếu không có -i thì sẽ gọi /etc/ansible/hosts
- -a : command\_argument gửi kèm theo module mà ta đang gọi
- -u : user
- -k: nhập pass
- -v : debug option

***

- **Một số ví dụ:**
  - `ansible all -m ping` (gọi ping toàn bộ các hosts trong /etc/ansible/hosts) 
  - `ansible apiserver -m command -a uptime`
  - `ansible apiserver -a uptime` (Default, ansible sẽ cho module = "command") 
  - `ansible apiserver -m shell -a 'top -bcn1 | head'` (giải thích: chạy lệnh shell ở remote client)
  - `ansible dbserver -m service -a "name=mysql state=restarted"` (restart mysql)

***
***
# Inventory

- Ansible có thể làm việc với một hoặc nhiều hệ thống một lúc. Để làm việc với nhiều máy chủ, ansible cần thiết lập kết nối tới các máy chủ đó. Điều này được thực hiện bằng SSH trên linux
- Thông tin về các hệ thống đích được lưu trong inventory file
- Nếu không tạo một  inventory file thì Ansible sử dụng một tệp sample inventory mặc định tại **/etc/ansible/hosts**
- Tệp kiểm kê định dạng giống như **ini**. Đơn giản là một số máy chủ liệt kê lần lượt. Cũng có thể nhóm các máy chủ khác nhau vào cùng 1 nhóm (nhập tên của nhóm trong dấu ngoặc vuông). Và cũng có thể thêm nhiều nhóm:

![17d8b19782744b66bbb61017cf94a420srSJdioGtWXDie43-25](https://user-images.githubusercontent.com/43572616/181207792-bd2282d2-b7c2-45c9-b434-d261200217b3.png)

- Ví dụ có một danh sách 4 máy chủ:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181207855-b42d5d50-f575-4c97-8194-d00db6ebac5b.png)



- Có thể đặt alias cho các máy chủ, ví dụ như web, db,… bằng cách sử dụng tham số **ansible_host**:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181207883-b184d41e-4dce-49fc-ad62-325632b0071f.png)



- Inventory Parameters:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181207912-cf9dac8b-dfe7-4f1f-aa49-386cb373fb96.png)

&emsp;- **ansible\_connection:** Kiểu kết nối, WinRM trên Windows, SSH trên Linux hoặc localhost

&emsp;- **ansible\_port:** xác định cổng kết nối (mặc định nó là cổng 22)

&emsp;- **ansible\_user:** xác định người dùng để thực hiện kết nối từ xa

&emsp;- **ansible\_ssh\_pass (linux) / ansible\_password (window):** Xác định mật khẩu ứng với ansible\_user muốn thực hiện kết nối.

![image](https://user-images.githubusercontent.com/43572616/181207949-f0234131-300e-476c-aa49-d6fc9015d420.png)


- Nhưng thiết lập mật khẩu dưới dạng văng bản như ansible\_ssh\_pass / ansible\_password không thực sự tốt. Lý do không đặt ssh key ở thư mục mặc định là do rất có thể có nhiều người quản trị cùng tham gia quản lý. Cách tốt hơn là "set up SSH key-based passwordless authentication" / "Set Up Passwordless SSH Login" như phần Individual Config
<br>
- Có thể đặt với localhost nếu muốn thực hiện trên local:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181207975-9ca5216a-409c-471a-a58c-2a60f498c931.png)



Không cần kết nối với máy chủ gì từ xa.

___

**Cấu hình file default inventory:** Ansible sử dụng một file inventory (danh sách các server) để kết nối với các server - Giống như với file hosts(/etc/hosts). 



File Inventory này có thể sẽ được tạo sẵn khi cài Ansible tại /etc/ansible/hosts:

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181208010-da10bb41-d4be-44e4-832b-9ce40c00050e.png)



- **VD1 cơ bản:**
```
# Sample Inventory File
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
db1 ansible_host=server4.company.com
```
***

- **VD2 thêm thông tin:**

|**Alias**|**Host**|**Connection**|**User**|**Password**|
| :- | :-: | :-: | :-: | :-: |
|web1|server1.company.com|SSH|root|Password123!|
|web2|server2.company.com|SSH|root|Password123!|
|web3|server3.company.com|SSH|root|Password123!|
|db1|server4.company.com|Windows|administrator|Password123!|

```
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!
```


&emsp; - **webx, db1** là alias

&emsp; - **ansible\_host=xxx** là IP/domain của server. Nếu sử dụng Port thì: `www.xxx.com:<port>`

***

- **VD3 thêm nhóm các web và database:**

```
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!

[web_servers]
web1
web2
web3

[db_servers]
db1
```


&emsp; - **web\_servers, db\_servers** là group của các server cần quản lý

***

- **VD4 tạo group cha:**

```
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!

[web_servers]
web1
web2
web3

[db_servers]
db1

[all_servers:children]
web_servers
db_servers
```
***

- **VD5:**

|**Server Alias**|**Server Name**|**OS**|**User**|**Password**|
| :-: | :-: | :-: | :-: | :- |
|sql\_db1|sql01.xyz.com|Linux|root|Lin$Pass|
|sql\_db2|sql02.xyz.com|Linux|root|Lin$Pass|
|web\_node1|web01.xyz.com|Win|administrator|Win$Pass|
|web\_node2|web02.xyz.com|Win|administrator|Win$Pass|
|web\_node3|web03.xyz.com|Win|administrator|Win$Pass|


|**Group**|**Members**|
| :-: | :-: |
|db\_nodes|sql\_db1, sql\_db2|
|web\_nodes|web\_node1, web\_node2, web\_node3|
|boston\_nodes|sql\_db1, web\_node1|
|dallas\_nodes|sql\_db2, web\_node2, web\_node3|
|us\_nodes|boston\_nodes, dallas\_nodes|

```
# Sample Inventory File

# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

# DB Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Groups
[db_nodes]
sql_db1
sql_db2

[web_nodes]
web_node1
web_node2
web_node3

[boston_nodes]
sql_db1
web_node1

[dallas_nodes]
sql_db2
web_node2
web_node3

[us_nodes:children]
boston_nodes
dallas_nodes
```
***

- **Gọi ansible:**
  - **Cấu trúc chung:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ansible [tên host] -m [tên module] -a [tham số truyền vào module]`

&emsp; -m là loại module
- **Lệnh ping:**
  - Nếu chưa khai báo user và pass trong inventory file:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`ansible [tên host] -m ping -u [user] -k`



&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;- **-k** là nhập password

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;-  Ở đây **-i** là đường dẫn hosts define. Nếu không dùng hosts define thì phải thêm -i rồi thêm đường dẫn

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;- Các option **-k -K -s** cần nhớ và mất nhiều thời gian, vì vậy có thể lưu thông tin luôn trong file inventory hoặc đặt "ssh-key" để quản lý Ansible tập trung và các client


- **Ví dụ:** 
  - Ping tất cả: `ansible all -m ping -i hosts`
  - Ping theo group: `ansible target -m ping -i hosts`
  - Ping từng host: `ansible target1 -m ping -i hosts`
<br>
- Ở các bước đầu tiên cần khai báo **host\_group để dễ gọi các khối server. Cần quy hoạch từng khối để dễ gọi lệnh ansible về sau.

***
***
# Playbook


- **Ansible playbooks** là orchestration language của Ansible, được sử dụng trong trong các file Playbook - nơi chứa các task được ghi dưới định dạng YAML, xác định những gì Ansible cần thực hiện, máy controller sẽ đọc các task này trong Playbook sau đó đẩy các lệnh thực thi tương ứng bằng Python xuống các máy con

<br>

- **Ví dụ:**

![image](https://user-images.githubusercontent.com/43572616/181213036-ded686e9-521c-4074-acfd-68ad83759acc.png)



![image](https://user-images.githubusercontent.com/43572616/181213057-3c6f413c-3699-44cc-8bb8-02c882b61da8.png)


- **Playbook:** là một file YAML
  - **Play:** là một tập các hành động (tasks) để run trên một nhóm các máy chủ  (hosts)
    - **Task:** một hành động duy nhất được thực hiện trên một máy chủ (host)
![image](https://user-images.githubusercontent.com/43572616/181213082-c5ce5d3e-1875-426d-876b-16d6d189c079.png)

***

- **Ví dụ:**

![image](https://user-images.githubusercontent.com/43572616/181213118-3c379f18-e0c6-41ce-ae48-db1da883ffe3.png)

- Một Play tên là "Play 1". Để chạy một tập các task trên localhost

***

- **Ví dụ 2:** Chia làm 2 plays riêng biệt:

![image](https://user-images.githubusercontent.com/43572616/181213150-312147ee-ebb2-40c1-8a90-25354234a840.png)

- Playbook này chứa 2 plays riêng biệt được phân cách bởi dấu gạch đầu dòng
- Thứ tự của name, hosts, tasks không quan trọng, có thể đổi chỗ

***

- **Playbook format:**

![image](https://user-images.githubusercontent.com/43572616/181213194-504b302c-6960-4c5e-963b-e00a29cb2b54.png)

***

- **Ví dụ 3:** cài Apache2 + deploy đơn giản .config file , deploy .html file

```
- hosts: servertest 
  become: true
  tasks:

  ########## Cài đặt gói tin httpd và start .
  - name: Install HTTP
    yum: name=httpd state=latest
  - name: Start HTTPD after install
    service: name=httpd state=started

########### Deploy config
#backup
  - name: Backup config HTTP (backup from client)
    command: cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.backup1

#Deploy
  - name: Deploy config httpd
    template:
     src: "/etc/ansible/config/httpd.conf"
     dest: "/etc/httpd/conf/httpd.conf"
     owner: root
     group: root
     mode: 0644

########### Đẩy code về client
  - name: Deploy web file
    template:
     src: "/etc/ansible/config/index.html"
     dest: "/var/www/html/index.html"

########### Khởi động lại apache để áp dụng config
  - name: Start HTTPD after install
    service: name=httpd state=restarted
```

Dưới task sẽ là các module để chạy. **Module yum** : để install gói tin http. **Module service**: để chạy lệnh "service httpd start". **Module command**: để chạy lệnh trên client. **Modudle Template**: là copy file từ ansible server tới client. Ngoài ra còn gán biến cho các file được copy.

***

- **Ví dụ 4:**

You are assigned a task to restart a number of servers in a particular sequence. The sequence and the commands to be used are given below. Note that the commands should be run on respective servers only. Refer to the inventory file and update the playbook to create the below sequence.

```
1. Stop the web services on web server nodes - service httpd stop
2. Shutdown the database services on db server nodes - service mysql stop
3. Restart all servers (web and db) at once - /sbin/shutdown -r
4. Start the database services on db server nodes - service mysql start
5. Start the web services on web server nodes - service httpd start
```

```
-
    name: 'Stop the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'
-
    name: 'Shutdown the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Shutdown the database services on db server nodes'
            command: 'service mysql stop'
-
    name: 'Restart all servers (web and db) at once'
    hosts: all_nodes
    tasks:
        -
            name: 'Restart all servers (web and db) at once'
            command: '/sbin/shutdown -r'
-
    name: 'Start the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Start the database services on db server nodes'
            command: 'service mysql start'
-
    name: 'Start the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Start the web services on web server nodes'
            command: 'service httpd start'
```

***

- **Chạy playbook:**

`ansible-playbook <command>`

`ansible-playbook <playbook\_name>.yml`

***
***
# Module

## Nhóm Module


- Ansible Module là một khối/đơn vị xử lý của một task (task thì sẽ thêm các biến, tham số vào nữa để xử lý trên remote server
 
- Trong Ansible, thay vì nói: "Ansible, hãy thực hiện lệnh này!". thì nói: "Ansible, hãy thực thi module này và cho phép nó chạy bất kỳ lệnh nào nó cần để hoàn thành công việc!".



- Các module Ansible được phân loại thành các nhóm khác nhau dựa trên chức năng. Một số **nhóm Module**:

***

- **System:** Là nhóm các modules thực hiện hành động trên một hệ thống, Ví dụ như sửa đổi nhóm/người dùng, iptable, cấu hình tường lửa, bắt đầu, dừng hoặc khởi động lại các dịch vụ trong một hệ thống,…

  ![image](https://user-images.githubusercontent.com/43572616/181214805-112e713e-9f5b-497e-9d3a-0b6458e86350.png)



- **Commands:** là nhóm các modules thực hiện lệnh hoặc tập lệnh trên máy chủ

  ![image](https://user-images.githubusercontent.com/43572616/181214838-a3cadb23-955b-41a9-9441-10e038a9d1fd.png)



- **Files:** nhóm các modules giúp làm việc với các tệp tin. Ví dụ như modules ACL (Access Control List) để đặt hoặc truy xuất thông tin ACL trên các files

  ![image](https://user-images.githubusercontent.com/43572616/181214857-58153b28-aaf6-4f88-bb84-c4c13699760d.png)



- **Database:** nhóm các Modules hỗ trợ làm việc với các cơ sở dữ liệu như MongoDB, MySQL, MsSQL,… Thêm hoặc xóa hoặc chính sửa cấu hình cơ sở dữ liệu

  ![image](https://user-images.githubusercontent.com/43572616/181214885-49569837-fbe2-4028-9807-d185db5ff906.png)



- **Cloud:** nhóm nhiều modules dành cho các nhà cung cấp đám mây khác nhau như Amazon, Azure, Docker, Google,…

  ![image](https://user-images.githubusercontent.com/43572616/181214909-04f9ac13-f21f-40ab-bcfb-c0dbc884c053.png)



- **Windows:** các modules giúp sử dụng Ansible trên môi trường window như: Win\_copy để copy files, Win\_command để thực thi một lệnh trên window,…

  ![image](https://user-images.githubusercontent.com/43572616/181214943-a5833733-8267-441e-8047-ae6b3374f74e.png)

***
## Modules details


- **Chạy các command modules đơn giản:** chỉ cần tạo 1 key-value của module

![image](https://user-images.githubusercontent.com/43572616/181215424-83522eea-68b0-4699-959e-8d4fd696884d.png)

***

**Chạy local script** trên nhiều máy chủ
  - Ansible sẽ tự động copy script sang tất cả máy chủ remote sau đó chạy chúng trên các máy chủ đó

![image](https://user-images.githubusercontent.com/43572616/181215518-92f757cd-f094-424c-9017-dbd19beeccb2.png)
![image](https://user-images.githubusercontent.com/43572616/181215550-d6a49490-c8a5-446b-9b8c-7e0c6bad0c61.png)



- **Ví dụ:** Update the playbook with a play to Execute a script on all web server nodes. The script is located at **/tmp/install\_script.sh**

```
-
    name: Execute a script on all web server nodes
    hosts: web_nodes
    tasks:
        -
            name: Execute a script on all web server nodes
            script: /tmp/install_script.sh
```

***
**Chạy các service:**

Đối với các node client là Unix/Linux, service module là giúp kiểm soát các service chạy trên các server. Giống với các module khác, service module cũng đi kèm với một số tham số và các tham số này có các tùy chọn riêng hoặc giá trị phù hợp. Sử dụng các tham số này và các giá trị bắt buộc, có thể quản lý các service với các chức năng như stop, start, reload, ... trên các node client.

![image](https://user-images.githubusercontent.com/43572616/181215867-4efbd3d5-9e72-4619-bde1-a0f5fa99cb55.png)



- **Ví dụ:**

```
- name: Start service httpd, if not running
  service:
    name: httpd
    state: started

- name: Stop service httpd, if running
  service:
    name: httpd
    state: stopped

- name: Restart service httpd, in all cases
  service:
    name: httpd
    state: restarted

- name: Reload service httpd, in all cases
  service:
    name: httpd
    state: reloaded

- name: Enable service httpd, and not touch the running state
  service:
    name: httpd
    enabled: yes
```

- **Ví dụ** Update the playbook to add a new task to `start httpd services` on all web nodes

```
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:            
        - 
            name: start httpd services
            service:
                name: httpd
                state: started
```


- **Idempotency:** Nếu service chưa bắt đầu thì ansible sẽ khởi động nó. Nếu đã khởi động trước đó rồi thì ansible không làm gì thêm.

![image](https://user-images.githubusercontent.com/43572616/181216090-50e0f48d-08b3-423f-8120-1e1cd1da43a6.png)

***
**Package management:** Đây là module được thiết kế cho các trình quản lý gói (package management) phổ biến như: apt, dnf,... cho phép cài đặt các package trên hệ thống. Ngoài các chức năng cụ thể của từng trình quản lý gói thì hầu hết các trình quản lý gọi đều có thể install, update, upgrade, downgrade, remove, và list các packages.

- **Ví dụ** install gói httpd trên CentOS/RHEL 8 sử dụng trình quản lý gói dnf:

```
- hosts: all
  user: ansible
  become: True
  tasks:
    - name: Install httpd package
      dnf:
        name: httpd
        state: present
        update_cache: True
```

***
**File:** Trong Ansible, có nhiều các module làm việc với tệp tin, thư mục, links trên các node đích (node client) như copy, template, file,... thường được sử dụng. Trong đó File module giúp quản lý tập tin và các thuộc tính của nó. Ngoài việc tạo, xóa, xác định vị trí của tệp tin file module cũng đặt các quyền và quyền sở hữu hay thiết lập symlinks cho tệp tin

- **Ví dụ:**

```
- hosts: all
  user: ansible
  become: True
  tasks:
    - name: Change file ownership, group and permissions
      file:
        path: /etc/thuntt.config
        owner: thuntt
        group: thuntt
        mode: '0644'

    - name: Create an insecure file
      file:
        path: /viblo
        owner: root
        group: root
        mode: '1777'

    - name: Create a symbolic link
      file:
        src: /file/to/link/to
        dest: /path/to/symlink
        owner: thuntt
        group: thuntt
        state: link
```

***
**Template:** Template trong Ansible là một tệp chứa các tham số cấu hình nhưng các giá trị động (có thể thay đổi) - được cung cấp dưới dạng biến. Trong quá trình thực thi playbook, các biến có thể được thay thế bằng các giá trị tùy biến. Ngoài ra, có thể có các câu lệnh có điều kiện, vòng lặp, viết macro, bộ lọc để chuyển đổi dữ liệu, thực hiện các phép tính số học,... Các file template thường sẽ có phần mở rộng là .j2. Các biến trong tệp template sẽ được ký hiệu bằng dấu ngoặc nhọn kép, '{{biến}}'



- **Ví dụ** file template ***hello\_world.j2***:

```
{{ variable_to_be_replaced }}
This line won't be changed
Variable given as inline - {{ inline_variable }}
```


Playbook:

```
- hosts: all
  vars:
    variable_to_be_replaced: 'Hello world'
    inline_variable: 'hello again'
  tasks:
    - name: Ansible Template Example
      template:
        src: hello_world.j2
        dest: /Users/mdtutorials2/Documents/Ansible/hello_world.txt
```


Ouput ***hello\_world.txt***:

```
Hello world
This line won't be changed
Variable given as inline - hello again
```

***
**lineinfile:** dùng để tìm một dòng trong file, thay thế nó hoặc thêm vào nếu trong file không tồn tại

- **Ví dụ:**

![image](https://user-images.githubusercontent.com/43572616/181216639-73f758d2-5ae2-4fd5-82bb-bef6ec2b200e.png)



Output:
![image](https://user-images.githubusercontent.com/43572616/181216670-9f8dc9dd-a61e-4417-894f-8724ba591a74.png)



- **Ví dụ:** Add a new task in the beginning to add an entry into /etc/resolv.conf file for hosts. The line to be added is nameserver 10.1.250.10

```
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: add a new task
            lineinfile:
                path: /etc/resolv.conf
                line: nameserver 10.1.250.10
```

***
**Copy:** sao chép một tệp tin từ Ansible server (Management node) đến các node đích (client node)

- **Ví dụ:**

```
- name: copy file from local to remote with owner, group and file permissions (octal)
  copy:
    src: test_file
    dest: $HOME/test_file
    owner: thuntt
    group: thuntt
    mode: 0644

- name: copy file from local to remote with owner, group and file permissions (octal)
  copy:
    src: test_file
    dest: $HOME/test_file
    owner: thuntt
    group: thuntt
    mode: 0644

- name: copy file from local to remote with root as the owner (become required)
  copy:
    src: test_file
    dest: "/home/{{ ansible_user }}/test_file"
    owner: root
    group: root
    mode: 0644
  become: true
```

***
**Shell:** shell module được sử dụng để chạy các lệnh thông qua shell (/bin/sh) trên các máy đích từ xa. Module này nhận các lệnh làm đầu vào cùng với một tập hợp các đối số.

- **Ví dụ:**

```
- name: Execute the command in remote shell; stdout goes to the specified file on the remote.
  shell: somescript.sh >> somelog.txt

- name: Change the working directory to somedir/ before executing the command.
  shell: somescript.sh >> somelog.txt
  args:
    chdir: somedir/
```

***
**User:** quản lý thông tin và các thuộc tính của người dùng

- **Ví dụ:** create a new web user. 

&emsp;&emsp;&emsp;User details to be used are given below:

&emsp;&emsp;&emsp;&emsp;**Username:** web\_user

&emsp;&emsp;&emsp;&emsp;**uid:** 1040

&emsp;&emsp;&emsp;&emsp;**group:** developers

```
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Create a new user'
            user:
                name: web_user
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
```

***
***
# Conditionals and Register


- **Ví dụ 1:** Nếu tuổi trên 22 thì in ra màn hình là "Tôi đã tốt nghiệp" và ngược lại nếu tuổi dưới 22 thì in là "Tôi chưa tốt nghiệp". Lúc này sẽ sử dụng thuộc tính when mà ansible cung cấp để giới hạn phạm vi chạy của câu lệnh.

```
#Simple playbook.yml
-
  name: Toi da tot nghiep chưa
  hosts: localhost
  vars:
    age: 25
  tasks:
    -
      command: echo "Toi chua tot nghiep"
      when: age < 22          
    -                     
      command: echo "Toi da tot nghiep"                     
      when: age >= 22
```


- **Ví dụ 2:** Nếu cài đặt trên Debian thì dùng lệnh apt, nếu trên Redhat thì dùng yum:

![image](https://user-images.githubusercontent.com/43572616/181218239-f636221c-118f-471c-846b-88a09855e214.png)

***
**Register:** Ansible còn cung cấp một thuộc tính là register. Register giúp nhận kết quả trả về từ một câu lệnh. Sau đó có thể dùng kết quá trả về đó cho những câu lệnh chạy sau đó.
  - **Ví dụ:** kiểm tra trạng thái của service httpd, nếu start thất bại thì gửi mail thông báo cho admin

```
#Sample ansible playbook.yml
-
  name: Check status of service and email if its down
  hosts: localhost
  tasks:
    - command: service httpd status
      register: command_output

    - mail:
        to: Admins 
        subject: Service Alert
        body: "Service is down"
      when: command_output.stdout.find("down") != -1
```
&emsp;&emsp;&emsp;Nhờ vào thuộc tính register, kết quả trả về sẽ được chứa vào biến *command\_output*. Từ đó sử dụng stdout.find để tìm chữ "down" có xuất hiện trong nội dung command\_output không. Nếu không tìm thấy thì kết quả sẽ là -1, nếu có thì thực hiện lệnh gửi mail

***
**Conditionals in Loop:**

Thay vì chỉ cài 1 package:

![image](https://user-images.githubusercontent.com/43572616/181218549-d421082d-a3a3-44af-ad6b-083cf67489e5.png)

Thì giờ có 1 danh sách các packages cần cài đặt:

![image](https://user-images.githubusercontent.com/43572616/181218568-c9d0c3bb-8f89-43cc-bf2d-2c426630458f.png)

Cài đặt gói chỉ khi required được kích hoạt (= True)

*playbook.yml:*

![image](https://user-images.githubusercontent.com/43572616/181218602-1fcbaddf-fabf-4129-87ae-92335b299560.png)



Vòng lặp loop thực tế là 3 tasks khác nhau, mỗi task để cài 1 package và được gọi là 1 item. Item chứa thông tin các chi tiết trong gói tương ứng:

![image](https://user-images.githubusercontent.com/43572616/181218645-1c38c1f1-875b-4b08-8bb9-715bc7cdbcad.png)

***
***
# Loops

- **Ví dụ** cần tạo nhiều user, bình thường sẽ phải viết:

![image](https://user-images.githubusercontent.com/43572616/181218996-d38e3750-6390-4a4b-a571-05f7985e692f.png)



Nếu tạo đến hàng trăm user thì sẽ rất có vấn đề. Vì vậy ta dùng loop:

![image](https://user-images.githubusercontent.com/43572616/181219029-b834f006-633e-4a78-bdca-0e1b3c774eb9.png)



Mỗi lần chạy, nó lưu giá trị của từng thành phần trong biến item

***

- **Một cách viết khác phổ biến hơn là dùng with\_\*:**

|![image](https://user-images.githubusercontent.com/43572616/181219146-0ca291dc-5893-4ae7-911e-ef68f6d5a8dd.png)|![image](https://user-images.githubusercontent.com/43572616/181219198-13dbb8fd-5801-47fa-ab69-8c7b6acdcb92.png)|
| :- | :- |


*with\_items* là một lệnh lặp, thực thi cùng một tác vụ nhiều lần.

Có nhiều lệnh lặp with\_\*:

![image](https://user-images.githubusercontent.com/43572616/181219322-6b7d4c24-4931-43bb-ae94-49332d3770a7.png)

![image](https://user-images.githubusercontent.com/43572616/181219380-60f77f2d-0937-4ff8-917b-62744712e87a.png)

***

- **Ví dụ:** Apply a loop directive (with\_items) to the task to print all fruits defined in the fruits variable.

```
#Sample ansible playbook.yml
-
    name: 'Print list of fruits'
    hosts: localhost
    vars:
        fruits:
            - Apple
            - Banana
            - Grapes
            - Orange
    tasks:
        -
            command: 'echo "{{item}}"'
            with_items: "{{fruits}}"
```

***
- **Ví dụ:** install multiple packages using yum module

```
#Sample ansible playbook.yml
-
    name: 'Install required packages'
    hosts: localhost
    vars:
        packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt
    tasks:
        -
            yum: 'name={{item}} state=present'
            with_items: "{{packages}}"
```

***
***
# Roles

- Trong ansible, admin sẽ "gán vai trò" cho máy chủ mới (blank servers) thành một data server, webserver, Redis messaging server, máy chủ sao lưu,…

![image](https://user-images.githubusercontent.com/43572616/181220582-8a9ad7da-5d8f-4f84-9ad1-a0b7a6ef5593.png)

  bằng một list các cài đặt và config trên server mới đó:
![image](https://user-images.githubusercontent.com/43572616/181220606-db52b7c9-4962-432e-8ae3-25efa868bf74.png)

<br>

- Với ví dụ setup một máy mysql, có playbook để thực hiện một list/kịch bản các cài đặt & config đó:

![image](https://user-images.githubusercontent.com/43572616/181220623-ec21a389-292c-479d-88e2-05afd305e861.png)

Quy trình cài mysql trong playbook này có thể chia sẻ với nhiều người đang setup một máy tương tự

Vì thế cần đóng gói nó lại thành 1 role để sử dụng sau:

![image](https://user-images.githubusercontent.com/43572616/181220655-8350d992-8bcb-4165-8685-9105372a4095.png)


Lần tới chỉ cần gán role đã tạo vào playbook:

![image](https://user-images.githubusercontent.com/43572616/181220689-25d9b250-a6e7-4bb7-bc68-d9c660cff759.png)


- Roles cũng giúp tổ chức code trong Ansible

![image](https://user-images.githubusercontent.com/43572616/181220727-2569b605-6472-4e3b-a4b1-e940d93615b4.png)



- Và chia sẻ với những người khác qua <https://galaxy.ansible.com/>

***
## Definition

- Nếu có nhiều server hay nhiều group server và mỗi server thực thiện những tasks riêng biệt. Và khi này nếu viết tất cả vào cùng một file playbook thì khá xấu code và khó để quản lý. Ansible đã cung cấp sẵn chức năng roles, về đơn giản nó sẽ giúp phân chia khu vực với nhiệm vụ riêng biệt.



- Ví dụ có một kịch bản cấu hình tường lửa cho group server về web, có setup\_application.yml:

```
# Simple Ansible setup_application.yml
-
  name: Set firewall configurations
  hosts: web
  vars:
    http_port: 8081
    snmp_port: 160-161
    inter_ip_range: 192.0.2.0
    
  tasks:
    - firewalld:
        service: https
        permanent: true
        state: enabled

    - firewalld:
        port: "{{ http_port }}"/tcp
        permanent: true
        state: disabled

    - firewalld:
        port: "{{ snmp_port }}"/udp
        permanent: true
        state: disabled

    - firewalld:
        source: "{{ inter_ip_range }}"/24
        zone: internal
        state: enabled
```


- Bây giờ sẽ cắt nhỏ file playbook này ra thành những file có chức năng riêng biệt. Trước khi cắt file playbook nhỏ gọn lại, cần tạo cấu trúc thư mục để ansible nhận biết được các thành phần đã khai báo:
![image](https://user-images.githubusercontent.com/43572616/181221554-0bf27485-fd23-456d-b7f7-2e774fe2d6af.png)

***
## Create roles


- Đầu tiên, tạo cấu trúc thư mục cho role, Ansible Galaxy có công cụ tạo ra một cấu trúc thư mục cho role cơ bản:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ansible-galaxy init <tên role>`

![image](https://user-images.githubusercontent.com/43572616/181221860-b98786d7-d384-43ee-bfde-bc9bf8dc25f2.png)

- Đưa code trong playbook vào các thư mục tương ứng:
![image](https://user-images.githubusercontent.com/43572616/181221887-99957bed-b7ed-4cbd-9632-40c2ed772875.png)

- Trong project chưa playbook.yml, tạo thư mục roles và đưa cấu trúc trên vào:
![image](https://user-images.githubusercontent.com/43572616/181221920-a305ea57-ff19-464c-9dcf-4b64dfe47b1a.png)



- Hoặc có thể đưa vào thư mục mặc định của roles (nơi ansible mặc định tìm kiếm roles):

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`/etc/ansible/roles/`



Chỉnh sử thư mục roles mặc định ở file ansible config:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`/etc/ansible/ansible.cfg`

***
# Ansible Galaxy

- **Tìm/ chia sẻ roles** qua <https://galaxy.ansible.com/>

***
**Tìm roles:**
- Tìm trên giao diện web

&emsp;&emsp;&emsp;![image](https://user-images.githubusercontent.com/43572616/181222229-1befdae2-b34a-49b8-afc5-32c673efb849.png)


- Hoặc dùng lệnh:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ansible-galaxy search <tên role>`

![image](https://user-images.githubusercontent.com/43572616/181222354-d7ec09da-cc98-4fae-b58c-84b1ae9e1dd3.png)

***
**Cài đặt role:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ansible-galaxy install <tên role>`

![image](https://user-images.githubusercontent.com/43572616/181222453-cfd3e7bc-1747-4fd0-9799-067d2b7a963f.png)



- **Từ đó viết playbook sẽ gọn và nhanh hơn:**

![image](https://user-images.githubusercontent.com/43572616/181222477-c716eae3-6e12-4df7-90a0-d5535dbeea6d.png)



![image](https://user-images.githubusercontent.com/43572616/181222490-a8f8b18e-7a23-43f2-a998-0cbf4025e9cf.png)

***

- **Liệt kê các role:**

![image](https://user-images.githubusercontent.com/43572616/181222517-92ccd17f-9473-482d-9cfa-e7ae963d36a0.png)

***
***
# Terms

- **CLI  - Command Line Interface:** giao diện dòng lệnh. Ngược lại là **GUI - Graphical User Interface,** là giao diện sử dụng ở dạng đồ họa.



- **Kiến trúc/giải pháp/cơ chế Agentless:** thông tin được truyền hoặc được thu thập từ các máy tính/hệ thống mà không cần phải cài thêm phần mềm đặc biệt nào (proprietary agents). Thực hiện bằng cách sử dụng chính những phương thức, ứng dụng có sẵn trên máy tính/hệ thống (bao gồm cả hệ điều hành và các native components)
 
- **Playbook:** Là file chứa các task được ghi dưới định dạng YAML. Máy controller sẽ đọc các task này trong Playbook sau đó đẩy các lệnh thực thi tương ứng bằng Python xuống các máy con
 
- **Orchestration:** một thuật ngữ rộng dùng để chỉ việc lên lịch container, quản lý cụm và khả năng cung cấp các máy chủ bổ sung.



- **batteries included:** muốn nói về việc khả năng sử dụng của một ứng dụng/sản phẩm khi nó được đóng gói/đi cùng với tất cả những thành phần cần thiết để có thể sử dụng một cách đầy đủ
 
- **Symlink (liên kết tượng trưng)** là các file liên kết trực tiếp đến những file khác. Trong những hệ thống Linux, symlink đóng vai trò là các shortcut
