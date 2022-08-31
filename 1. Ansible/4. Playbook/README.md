# **Playbook**


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

`ansible-playbook <playbook_name>.yml`
