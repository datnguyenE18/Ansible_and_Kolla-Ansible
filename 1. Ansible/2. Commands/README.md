# **Commands**


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
