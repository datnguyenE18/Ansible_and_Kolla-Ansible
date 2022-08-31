# **Begin**

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
