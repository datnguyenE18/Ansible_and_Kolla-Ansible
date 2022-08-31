# Table of Content

- [Table of Content](#table-of-content)
  - [Kolla-Ansible](#kolla-ansible)
  - [[kolla] Cấu trúc thư mục](#kolla-cấu-trúc-thư-mục)
  - [Loadbalancer role](#loadbalancer-role)
  - [Ví dụ luồng chạy Loadbalancer](#ví-dụ-luồng-chạy-loadbalancer)

## Kolla-Ansible

- **Cloud Computing:** Điện toán đám mây (Cloud Computing) là việc cung cấp các tài nguyên máy tính cho người dùng tùy theo mục đích sử dụng thông qua kết nối Internet. Nguồn tài nguyên đó có thể là bất kì thứ gì liên quan đến điện toán và máy tính, ví dụ như phần mềm, phần cứng, hạ tầng mạng cho đến các máy chủ và mạng lưới máy chủ cỡ lớn. Đơn giản thì Điện toán đám mây là bước kế tiếp của Ảo hóa. Ảo hóa phần cứng, ảo hóa ứng dụng là thành phần quản lý và tổ chức, vận hành các hệ thống ảo hóa trước đó
</br>

- OpenStack là một phần mềm mã nguồn mở, dùng để triển khai Cloud Computing, bao gồm private cloud và public cloud (nhiều tài liệu giới thiệu là Cloud Operating System):

- ![d576bef86a9bb9f3a2ca6278022976d2342d8c32](https://user-images.githubusercontent.com/43572616/185066325-d97fcaef-cec5-48a7-925d-d6c10e212feb.jpg)

  - Phía dưới là phần cứng máy chủ đã được ảo hóa để chia sẻ cho ứng dụng, người dùng. Trên cùng là các ứng dụng của người dùng, tức là các phần mềm mà user sử dụng

  - OpenStack là phần ở giữa 2 phần trên, trong OpenStack có các thành phần, module khác nhau nhưng trong hình có minh họa các thành phần cơ bản như Dashboard, Compute, Networking, API, Storage …

- Kolla là 1 project lớn của Openstackcung cấp các công cụ, images (là các OpenStack services và các thành phần hỗ trợ) cho docker containers, ansible playbook, Kubernetes template để triển khai, vận hành Openstack cloud
</br>

- Ý tưởng của Project Kolla là triển khai Openstack trong môi trường Container, tự động triển khai Openstack bằng Kolla Ansible. Qua đó chỉ với 1 vài thao tác là có môi trường Openstack để sử dụng. Hơn nữa, Project Kolla cũng cung cấp sẵn các giải pháp về giám sát, HA, Rolling Upgrades … cho Openstack
  - Các services như Keystone, glance, horizon, neutron,.... sẽ được chạy trên containers.
  - Instance được tạo ra, sẽ chạy trên node compute giống với cài đặt như bình thường.
</br>

- Về mặt công nghệ, dự án Kolla phát triển dựa trên hai công cụ chính đó là Docker và Ansible. Trong đó, việc triển khai các dịch vụ sẽ được tự động hóa bằng Ansible. Với Ansible thì các thao tác tay thủ công lặp lại đã gần như được giải quyết hoàn toàn
</br>

- **Ưu điểm khi sử dụng kolla:**
  - Đơn giản hóa việc triển khai OpenStack: nhanh chóng, dễ dàng.
  - Dễ dàng mở rộng (scale), nâng cấp (upgrade) OpenStack.
  - Chia nhỏ các services của OpenStack thành micro service (Docker containers). Mỗi micro service sẽ độc lập trong deployment, upgrading và scaling,…
</br>

- Kolla Ansible là một deliverable project được tách ra từ Kolla project. Nó triển khai các dịch vụ OpenStack và các thành phần cơ sở hạ tầng bằng các Docker container.
</br>

- **Có thể cài đặt Kolla với 2 phương pháp khác nhau:**
  - Cài đặt chạy các services openstack trên nền docker thuần.
  - Cài đặt chạy các services openstack trên nền kubernetes sử dụng công nghệ containers là docker.

***

## [kolla] Cấu trúc thư mục

- **Kolla Ansible triển khai các container cho các thành phần cơ sở hạ tầng:**
  - **Collectd, Telegraf, InfluxDB, Prometheus, and Grafana**, để giám sát hiệu suất.
  - **Elasticsearch and Kibana** , để tìm kiếm, phân tích và trực quan hóa log messages.
  - **Etcd**: một hệ thống phân tán, mã nguồn mở, dùng để lưu trữ các giá trị key-value, lưu trữ toàn bộ cấu hình, trạng thái và metadata của Kubernetes Cluster.
  - **Fluentd** , một bộ dữ liệu mã nguồn mở cho lớp unified logging
  - **Gnocchi**: một CSDL time-series
  - **HAProxy and Keepalived** : high availability cho các dịch vụ và các endpoints của chúng
  - **MariaDB and Galera Cluster** dành cho cơ sở dữ liệu MySQL có HA.
  - **Memcached** a distributed memory object caching system.
  - **Open vSwitch** for use with Neutron.
  - **RabbitMQ** như một chương trình hỗ trợ giao tiếp giữa các dịch vụ.
  - **Redis** một kho lưu trữ cấu trúc dữ liệu trong bộ nhớ.
  - **Zookeeper** một máy chủ mã nguồn mở cho phép tập duy trì thông tin cấu hình, đặt tên, cung cấp sự đồng bộ phân tán, cung cấp dịch vụ nhóm. Nói cách khác, phối hợp phân tán cho các ứng dụng phân tán
</br>

- **Các thư mục:**
  - **ansible -** Chứa các Ansible playbook để deploy các dịch vụ OpenStack và các thành phần cơ sở hạ tầng bằng Docker container
  - **contrib -** Chứa các kịch bản demo cho Heat, Magnum, Tacker và môi trường phát triển cho Vagrant
  - **doc -** Chứa documentation.
  - **etc -** Contains a reference etc directory structure which requires configuration of a small number of configuration variables to achieve a working All-in-One (AIO) deployment.
  - **kolla\_ansible -** chứa password generation script.
  - **releasenotes -** Chứa bản releasenote của tất cả các tính năng được thêm vào Kolla Ansible
  - **specs -** Contains the Kolla Ansible communities key arguments about architectural shifts in the code base.
  - **tests -** Chứa các tool để test chức năng
  - **tools -** Chứa các tool để tương tác với Kolla Ansible.
  - **zuul.d -** Chứa các định nghĩa về project gate job

***

## Loadbalancer role

- **Cấu trúc chung của một Role:**
  - **Task:** Chứa các file yaml định nghĩa các nhiệm vụ chính khi triển khai.
  - **Handles:** Chứa các handler được sử dụng trong role
  - **Files:** chứa các file dc sử dụng bởi role, ví dụ như các file ảnh.
  - **Templates:** chứa các template file được sử dụng trong role, ví dụ như các file configuration... Các file này có đuôi \*.j2, sử dụng jinja2 syntax
  - **Vars:** định nghĩa các variable được sử dụng ở trong roles
  - **Defaults:** Định nghĩa các giá trị default của các variable được sử dụng trong roles. Nếu variable không được định nghĩa trong thư mục vars, các giá trị default này sẽ được gọi.
  - **Meta:** thư mục này chứa meta data của roles
</br>

- **Cấu trúc thư mục của loadbalancer role:**
![image](https://user-images.githubusercontent.com/43572616/185066581-5f10be70-74db-4596-bd4d-3f7ba0916ce6.png)
</br>

- **vars/main.yml:** định nghĩa các variable được sử dụng ở trong roles. Ở đây đã khai báo tên của project:

    ```sh
    ---
    project_name: "loadbalancer"
    ```

</br>

- **defaults/main.yml:** Chứa các variable mà người dùng hầu như không hoặc hiếm khi thay đổi như:
  - Cấu hình cơ bản của container:

    ```sh
    loadbalancer_services:
    haproxy:
        container_name: haproxy
        group: loadbalancer
        enabled: true
        image: "{{ haproxy_image_full }}"
        privileged: True
        volumes: "{{ haproxy_default_volumes + haproxy_extra_volumes }}"
        dimensions: "{{ haproxy_dimensions }}"
        healthcheck: "{{ haproxy_healthcheck }}"

    keepalived:
        container_name: keepalived
        group: loadbalancer
        enabled: "{{ enable_keepalived | bool }}"
        image: "{{ keepalived_image_full }}"
        privileged: True
        volumes: "{{ keepalived_default_volumes + keepalived_extra_volumes }}"
        dimensions: "{{ keepalived_dimensions }}"
    ```

- Sau đó là đến các Global Configuration, cấu hình phụ thuộc vào hệ thống, được định nghĩa ở **./ansible/group_vars/all.yml**:

  - **kolla_base_distro**
   Kolla base container image distribution. Options are **centos**, **debian**, or **ubuntu**. Default is **{{ os_distribution }}**.
</br>

  - **kolla_install_type**
   Kolla container image type: **binary** or **source**. Default is **source**.
</br>

  - **kolla_docker_namespace**
   Docker namespace to use for Kolla images. Default is **kolla**.
</br>

  - **kolla_docker_registry**
   URL of docker registry to use for Kolla images. Default is to use the value of **docker_registry** variable
</br>

  - **kolla_docker_registry_username**
   Username to use to access a docker registry. Default is not set, in which case the registry will be used without authentication.
</br>

  - **kolla_docker_registry_password**
   Password to use to access a docker registry. Default is not set, in which case the registry will be used without authentication.
</br>

  - **kolla_openstack_release**
   Kolla OpenStack release version. This should be a Docker image tag. Default is the OpenStack release name (e.g. **rocky**) on stable branches and tagged releases, or **master** on the Kayobe **master** branch.
</br>

  - **kolla_tag**
   Kolla container image tag. This is the tag that will be applied to built container images. Default is **kolla_openstack_release**.
</br>

- Và global configuration mở rộng, dùng để tối ưu hóa, cấu hình trực tiếp trên file

    ```sh
    haproxy_max_connections: 40000
    haproxy_processes: 1
    haproxy_process_cpu_map: "no"
    # Matches the mariadb 10000 max connections limit
    haproxy_defaults_max_connections: 10000
    ...
    ```

</br>

- **Templates:** Chứa các file cấu hình của HAProxy và Keepalived
</br>

- **handlers/main.yml:** chứa các tác vụ y như ‘tasks’ trong playbook nhưng khác ở chỗ ‘task‘ được tự động khởi chạy theo flow cấu hình hoặc theo ‘tag‘ còn ‘handler‘ thì không thể khởi chạy nếu không có tín hiệu (notify) gọi sử dụng.
</br>

  - Là danh sách một hoặc nhiều tác vụ chờ được gọi sử dụng từ task. Một tác vụ handler được khai báo cấu hình y hệt một task, nhưng yêu cầu phải có tên tác vụ rõ ràng để dùng khi gọi.
</br>

  - **notify** là keyword dùng để gọi một hoặc nhiều handler trong một task. Khi notify các tác vụ ‘handler‘ , thì các tác vụ sẽ luôn chạy theo thứ tự từ trên xuống đã được định nghĩa.
</br>

  - Cấu trúc chung của handlers:

    ```sh
    ---
    - hosts: vb
        ...

    tasks:
        ...
        notify:

    handlers:
        ...
    ```

- .
  - **Handler Listen:** để gom nhóm các tác vụ ‘handler‘ thành một group theo tên ‘listen‘. Chỉ cần gọi tên của ‘listen‘ được khai báo là có thể chạy toàn bộ các tác vụ ‘handler‘ con của nhóm ‘listen‘ đó. Các tác vụ ‘handler‘ trong một nhóm ‘listen‘ vẫn chạy theo thứ tự đã khai báo trong mục danh sách tác vụ ‘handler‘

  - **Task:** Chứa các file yaml định nghĩa các nhiệm vụ chính khi triển khai.

***

## Ví dụ luồng chạy Loadbalancer

- Ví dụ muốn deploy service, sẽ bắt đầu từ ***deploy.yml***:

    ```sh
    ---
    - import_tasks: config-host.yml

    - import_tasks: config.yml

    - import_tasks: check-containers.yml
    ```

</br>

- Bằng import_task, sẽ chạy lần lượt các file yaml từ trên xuống. Tiếp theo sẽ là ***config-host.yml***, ở đây có 2 tasks. 
  **Task thứ nhất:**

    ```sh
    ---
    - name: Setting sysctl values
    vars:
        should_set: "{{ item.value != 'KOLLA_UNSET' }}"
    sysctl:
        name: "{{ item.name }}"
        state: "{{ should_set | ternary('present', 'absent') }}"
        value: "{{ should_set | ternary(item.value, omit) }}"
        sysctl_set: "{{ should_set }}"
        sysctl_file: "{{ kolla_sysctl_conf_path }}"
    become: true
    with_items:
        - { name: "net.ipv4.ip_nonlocal_bind", value: 1 }
        - { name: "net.ipv6.ip_nonlocal_bind", value: 1 }
        - { name: "net.ipv4.tcp_retries2", value: "{{ haproxy_host_ipv4_tcp_retries2 }}" }
        - { name: "net.unix.max_dgram_qlen", value: 128 }
    when:
        - set_sysctl | bool
        - item.value != 'KOLLA_SKIP'
    ```

    Có module sysctl, quản lý và thay đổi các giá trị của sysctl 

</br>

  **Task thứ 2:**

```sh
- name: Load and persist keepalived module
import_role:
    name: module-load
vars:
    modules:
    - {'name': ip_vs }
```

Ở đây gọi đến một role khác có tên là module-load để chạy và khởi động module ip\_vs khi boot:
![image](https://user-images.githubusercontent.com/43572616/185071735-c2877143-0d60-472e-832a-a3ab1c69f2c0.png)

- Sau đó chạy đến ***config.yml***:
  - Ở đây ban đầu sẽ kiểm tra xem thư mục và file cấu hình có tồn tại không qua 2 task: "Ensuring config directories exist" và "Ensuring haproxy service config subdir exists"

  - Task "Copying over config.json files for services" sẽ copy các file .json.j2 từ Templates cho lần lượt các services. Sau đó sẽ gọi các tasks ở Handles thông qua notify để khởi động lại HAProxy và Keepalived container

  - Cuối cùng mới copy các file cấu hình khác của HAProxy và Keepalived cho từng services để hoàn tất quả trình config.

- Tiếp theo là check-containers.yml, ở đây có module kolla\_docker, để sử dụng Docker trong Kolla, dùng option compare\_container để check giá trị, env,… của các loadbalancer container. Sau đó khởi động lại tất cả các container bằng các tasks bên Handles
