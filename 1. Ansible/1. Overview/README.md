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
