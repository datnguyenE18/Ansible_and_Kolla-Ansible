# **Nhóm Module**


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
