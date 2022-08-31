# **Inventory**

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
- Ở các bước đầu tiên cần khai báo host_group để dễ gọi các khối server. Cần quy hoạch từng khối để dễ gọi lệnh ansible về sau.
