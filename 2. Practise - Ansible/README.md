# **Practise**

**Task:** Xây dựng hệ thống dịch vụ có cân bằng tải (load balancing) và tính khả dụng cao (highly available) sử dụng HAProxy + Keepalived. Tất cả được Deploy bằng Docker

Triển khai bằng Ansible

|**ansible_controller**|**192.168.23.142**|
| :- | :- |
|ansible_target_1|192.168.23.143|
|ansible_target_2|192.168.23.144|

Ở đây sử dụng trực tiếp tài khoản root trên các máy agent
***

## Tạo SSH key

### Tạo cặp key:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ssh-keygen -C "ansible@master"`

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; *(nhấn enter tất cả để chọn mặc định lưu cặp key tại /root/.ssh)*

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ![image](https://user-images.githubusercontent.com/43572616/183373168-3339a1f7-f341-476d-a3b7-239c83244866.png)

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ll /root/.ssh/`

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ![image](https://user-images.githubusercontent.com/43572616/183373192-92f8a6c8-3a26-4eed-9126-ab02950ba836.png)

### Copy public key sang các máy agent:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ssh-copy-id -i /root/.ssh/id\_rsa.pub root@192.168.23.143`

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ![image](https://user-images.githubusercontent.com/43572616/183373221-85967694-89d7-4a3a-85e0-d5231625de66.png)

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ssh-copy-id -i /root/.ssh/id\_rsa.pub root@192.168.23.144`

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ![image](https://user-images.githubusercontent.com/43572616/183373256-75cd6a1e-8b84-4d10-98c5-3bc3c8036f6f.png)

- **Thử kết nối ssh không cần password:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ssh root@192.168.23.143`

![image](https://user-images.githubusercontent.com/43572616/183373274-e9932804-91c8-4d06-8ee7-1f5c38b41cea.png)

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ssh root@192.168.23.144`

![image](https://user-images.githubusercontent.com/43572616/183373302-36c73dbe-a27b-4840-85ee-a70c0c3af0f7.png)

### Sử dụng ansible:

```sh
- 
  name: generate SSH key
  hosts: 127.0.0.1
  connection: local
  
  vars:
    ssh_key_filename: id_rsa
    agents:
      - 192.168.23.143
      - 192.168.23.144
    root_password: "12345"
    id_rsa_file: "/root/.ssh/id_rsa"

  vars_prompt:
    - name: "ssh_passphrase"
      prompt: "Enter the passphrase for the SSH key"
  
  tasks:

    - stat:
        path: "{{ id_rsa_file }}"
      register: op

    - name: Generate SSH key "{{ssh_key_filename}}" on master
      user:
        name: "root"
        generate_ssh_key: yes
        ssh_key_type: rsa
        ssh_key_bits: 4096
        ssh_key_file: /root/.ssh/{{ssh_key_filename}}
        ssh_key_passphrase: "{{ssh_passphrase}}"
        force: no

    - debug: 
        msg: "Key pair already exists. Using the same key."
      when: op.stat.exists

    - name: Copy public key to the agents
      command: sshpass -p "{{ root_password }}" ssh-copy-id -i "{{ id_rsa_file }}" root@"{{ item }}" -f -o StrictHostKeyChecking=no
      with_items:
        - "{{ agents }}"
```

- Tạo inventory file trước khi chạy playbook

## Cấu hình ansible

### Config inventory file:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `vi /etc/ansible/hosts`

```sh
web1 ansible_ssh_host=192.168.23.143 ansible_ssh_private_key_file=/root/.ssh/id_rsa ansible_ssh_user=root
web2 ansible_ssh_host=192.168.23.144 ansible_ssh_private_key_file=/root/.ssh/id_rsa ansible_ssh_user=root

[webservers]
web1
web2
```

- **Thử ping đến 2 máy:**

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ansible all -m ping`

![image](https://user-images.githubusercontent.com/43572616/183373736-e4ffeadb-113a-491a-abda-a46aa482a5e1.png)

=> Kết nối tốt

- Tạo playbook.yml tại /root:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `touch playbook.yml`

![image](https://user-images.githubusercontent.com/43572616/183373771-1fee1dfb-9bce-4d65-a893-8dd0f3002c99.png)

***

## Cài đặt Docker và Environment

### Cài đặt Docker và cấu hình môi trường trên các agent:

```sh
-
  name: "all webservers"
  hosts: webservers
  tasks:
      ###### Check installed docker
    - name: "Check Docker"
      command: 'rpm -qa | grep docker'
      register: docker_is_existed

    - mail:
        to: datnguyen532k@gmail.com 
        subject: Service Alert
        body: "Docker is existed!"
      when: docker_is_existed.stdout.find("docker") != -1

      ###### Install Docker & Docker-Compose
    - name: "update Yum"
      command: yum update -y
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "Install the yum-utils package"
      command: yum install -y yum-utils
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "Set up the stable repository"
      command: yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "Install Docker Engine"
      command: yum install -y docker-ce docker-ce-cli containerd.io
      when: docker_is_existed.stdout.find("docker") == -1

    - debug:
        var: docker_is_installed.stdout_lines

    - name: "Enable Docker"
      service:
        name: docker
        enabled: yes
      when: docker_is_existed.stdout.find("docker") != -1

    - name: "Start Docker"
      service:
        name: docker
        state: started
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "Install Docker-compose"
      command: yum install docker-compose-plugin -y
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "clear yum cache"
      command: yum clean all

    ###### Environment Config
    - name: "Enable ip_vs"
      command: "{{item}}"
      with_items:
        - yum install ipvsadm
        - modprobe ip_vs
        - ipvsadm
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "net.ipv4"
      command: "{{item}}"
      with_items:
        - sysctl -w net.ipv4.ip_forward=1
        - sysctl -w net.ipv4.ip_nonlocal_bind=1
      when: docker_is_existed.stdout.find("docker") == -1

    - name: "Add port"
      command: "{{item}}"
      with_items:
        - firewall-cmd --add-port=80/tcp
        - firewall-cmd --add-port=8404/tcp
      when: docker_is_existed.stdout.find("docker") == -1
```

***

## Setup Servers

```sh
-
  name: docker compose
  hosts: webservers
  tasks:
    - name: copy src to remote
      copy:
        src: ./src
        dest: .

    - name: Install on Ansible_Target_1
      command: docker compose up -d keepalived-a haproxy-a haproxy-b web-a web-b
      args:
        chdir: /root/src
      register: target1
      when: ansible_hostname == "ansible_target_1"
    
    - debug:
        var: target1.stdout_lines

    - name: Install on Ansible_Target_2
      command: docker compose up -d keepalived-b haproxy-a haproxy-b web-a web-b
      args:
        chdir: /root/src
      register: target2
      when: ansible_hostname == "ansible_target_2"
    
    - debug:
        var: target2.stdout_lines
```

***

## Cài đặt

- Copy (ghi đè) file hosts (trong project) vào /etc/ansible
- Và chạy playbook.yml trong project

***

## Role:

- cd vào LB_roles
- Chạy lệnh:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; `ansible-playbook -i inventory playbook.yml`
