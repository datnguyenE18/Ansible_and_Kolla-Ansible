# **Chạy modules cụ thể**


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

