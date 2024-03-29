    Docker	
	║   		─SSL─
	║	 	NGINX							 PM2
	║		│					React app	  │
	║       	├── react  ───────────────────────────────────────────────┤
	║		│   │							  │	
	╠═══════════════╪═══╧══ Dockerfile-setup-react		 		  │
	║		│							  │	
	║		│ 					First server	  │
	║		├── node   ───────────────────────────────────────────────┤
	║		│   ├─ api/						  │	
	║		│   │							  │
	╠═══════════════╪═══╧══ Dockerfile-setup-node	     			  │
	║		│							  │
	║		│					Second server     │
	║		├── node   ───────────────────────────────────────────────┤
	║		│   ├─ api/						  │	
	║		│   │							  │
	╠═══════════════╪═══╧══ Dockerfile-setup-node	     			  │
	║		│							  │
	║		│					Third server      │
	║       	└── node   ───────────────────────────────────────────────┤
	║			├─ api/						  │	
	║			│						  │	
	╠═══════════════════════╧══ Dockerfile-setup-node	   		  │	
	║									  │ Mobile
	║									  └──────── react-native
	║											│
	╠═══════════════════════════════════════════════════════════════════════════════════════╧═ Dockerfile-setup-react
	║   mongo											  	
	║   ├─ run.sh 										  
	║   ├─ set_mongodb_password.sh
	╠═══╧══ Dockerfile-setup-mongo
	║
	╚═ docker-compose.yml


<!-- ==================================================================================================================== -->

## DOCKER

1. Docker là gì

Docker không phải là một ngôn ngữ lập trình hay Framework. Docker là một công cụ để build, chạy và deploy application của bạn lên bất cứ môi trường nào. Ở đây nói bất kì môi trường nào thì không đúng, bởi vì môi trường đấy sẽ cần phải cài đặt Docker để chạy được application của bạn. Công nghệ cốt lõi mà Docker sử dụng là Container Technology. Trước khi Docker ra mắt Container đã là một best-practice mà nhiều công ty to đã sử dụng. Tuy nhiên để tạo được một Container Bạn sẽ cần phải có kiến thức về Hệ điều hành và Security. Ngoài ra thì quá trình tạo rất phức tạp và dễ mắc phải sai lầm. Nhưng điều đấy đã hoàn toàn thay đổi khi Docker ra đời. Bạn có thể tạo và sử dụng Container Mà hoàn toàn không cần phải có kiến thức Về container technology. Tuy nhiên Container mà bạn tạo vẫn theo chuẩn của Best-Practice. Đây chính là lí do vì sao Docker lại trở nên phổ biến nhanh như vậy.

2. Lợi ích khi sử dụng Docker

Trước khi có docker thì để quản lí setup và config trên nhiều môi trường khác nhau rất phức tạp. 
Khi sử dụng docker
- Bạn sẽ tiết kiệm được thời gian quản lý setup và configuration
- Không phải lo lắng về Application Code sẽ được chạy ở hệ điều hành nào Ví dụ như là Ubuntu, CentOS hay Debian. Bởi vì chỉ cần môi trường đấy có Docker Runtime hay là đã được cài Docker. Bạn có thể chạy Application Code của bạn thông qua Docker Image

3. Một số use-case cơ bản khi sử dụng docker

- Prototype Application - Sandbox Environment 
- Continuous Delivery - DevOps CD Pipeline
- Microservice Pattern - Deploy Kubernetes

4. Các khác niệm quan trọng Docker

***`<Container>`***
- Container là một instance được sinh ra từ Docker Image
- Mỗi khi bạn chạy command, Mỗi Image có thể được chạy bởi nhiều Container
- Lấy ví dụ ở trong lập trình hướng đối tượng (OOP)
	- `<Image>`là class
	- `<Container>` sẽ là Object
- Mỗi container sẽ chạy tách biệt với nhau

***`<Image>`***
- [ ] https://docker-ghichep.readthedocs.io/en/latest/ghichep-docker-images/
- Image sẽ là template được sử dụng để tạo Container
- Trong Image sẽ chưa tất cả các Dependency Libraries, packages, configuration files và application code  để có thể chạy được application của bạn
- Một image sẽ được cấu thành bởi nhiều Layer (các lớp). Mỗi layer sẽ tương ứng với một thay đổi trên FileSystem tính từ Base Image mà bạn sử dụng

- ***`<Image layers>`***
- là cơ chế cực kỳ khi sử dụng docker vì docker sẽ tái sử dụng image layer để giảm thời gian build docker image cũng như là size curl của 1 docker image

***`<Volume>`***
- [ ] https://docker-ghichep.readthedocs.io/en/latest/volume/
- Volume trong Docker được dùng để chia sẻ dữ liệu cho container.

5. Docker Architecture

Ở trong Docker chúng ta có 2 thành phần chính là Docker Daemon và Docker Client:
- Docker Daemon là một Process chạy ngầm ở Background thường sẽ là Systemd Service. 
	- Docker Daemon sẽ cung cấp 1 REST API để Docker Client giao tiếp và thực hiện các tác vụ cần thiết.
	- Docker Daemon sẽ quản lý quyền truy cập và quản lý trạng thái của các container và image trên hệ thống.
	- Docker daemon mặc định sẽ sử dụng một UNIX Socket, mà chỉ User 'root' và các user thuộc Group 'docker' mới có thể access tới socket này
	- Ngoài ra bạn có thể đổi UNIX Socket này thành TCP Port để các application bên ngoài hệ thống có thể truy cập tới Docker Daemon.

- Docker Client: chính là các command mà bạn gõ vào Terminal
	- Docker Client sẽ giao tiếp với Docker Daemon bằng cách gửi HTTP Request tới REST API mà Docker Daemon cung cấp.

Ngoài Docker Daemon và Client chúng ta sẽ có một thành phần khác nữa gọi là Docker Registry. Đây là một service để lưu giữ các Docker Image. Nếu Docker Daemon của bạn có quyền truy cập tới một Docker Registry tải các Docker Image được lưu giữ ở trên Docker Registry đấy về máy tính của bạn. Điền hình chúng ta có Docker Hub
- Public Registry được quản lý bởi Docker (công ty). Ngoài ra bạn có thể sử dụng các Docker Registry được cung cấp từ các Cloud Provider - AWS, GCP, Azure
- Bạn cũng có thể cài đặt 1 Docker Registry - On-premis - Private Network trong công ty bạn

6. Giải thích Dockerfile

- [ ] https://docker-ghichep.readthedocs.io/en/latest/dockerfile/

```javascript
Dockerfile là một chuỗi instruction mà bạn định nghĩa
Dockerfile cũng liên quan đến 1 khái niệm gọi là infrastructer at code
Để viết dockerfile nên có kiến thức về LINUX COMMANDS và SHELL SCRIPTING
```

- FROM: chỉ định base image mà mình dùng - `<image-name>:<tag>`
	- best practice: nên chỉ định tag và trành dùng tag latest để duy trì ổn định cho app
- LABEL: dùng để add các metadata vào image
- ENV: tạo enviroment variable mà container sẽ sử dụng
- WORKDIR: chỉ định directory hay context để docker chặn các instruction nằm phía sau workdir ở tại directory này
- VOLUME: dùng để chỉ định 1 mount poin tức là 1 directory để lưu trữ dữ liệu bên trong container. Vì mặc định khi container bị xóa thì dữ liệu mà container đã lưu vào file system (databse chẳng hạn) sẽ bị mất theo. Vậy muốn lưu dữ liệu thì nên tạo VOLUME và lưu dữ liệu vào bên trong VOLUME để tái sử dụng
- EXPOSE: để chỉ định cổng port mà container sẽ chờ request tại port đó
- COPY: copy 1 file or host machine từ directory vào trong container
- ADD: chức năng gần giống COPY nhưng ADD có thể tải 1 file từ url mà bạn chỉ định bên trong container
- ARG: tùy chỉnh dockerfile để truyền vào dockerfile 1 số variable
- USER: nên tránh sử dụng user root
- RUN: chạy linux comand
	- best practice: khi 1 lệnh run chạy sẽ tạo ra 1 image layer, giả sử trong dockerfile có nhiều lệnh run, nếu được hãy gộp các lệnh run vào 1 để giảm thiếu số lượng image layer sinh ra. Nếu làm được sẽ tiết kiệm được size hay curl của image, giảm thời gian build docker image
- CMD: định nghĩ 1 command sẽ chạy chi container start
- ENTRYPOINT: giống CMD
> nhưng khi chạy song song CMD và ENTRYPOINT thì ENTRYPOINT thành command mà container chạy và CMD thành tham số cho command đấy

> Best-practice khi viết Dockerfile: Thứ tự viết các instruction chia làm 2 phần: ít thay đổi và hay thay đổi. Thì instruction ít thay đổi nên đặt phía trên và instruction hay thay đổi nên đặt phía dưới. Lý do làm vậy vì docker sẽ tiết kiệm thời gian sync lại các image layer cho nên sẽ tiết kiệm thời gian build lại image

7. Docker command

- [ ] https://docker-ghichep.readthedocs.io/en/latest/ghichep-lenh-docker/
- Build Dockerfile: 
	- docker image build -t `<image-name><context>`
	- vd: docker image build -t demo-backend .

- Check docker image on host machine
	- docker image ls

- Check image layers of every docker image
	- docker inspect image `<image-name>`
	- vd: docker inspect image demo-backend

- Run a command in a new container
	- docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

- Check docker container running on system
	- docker container ls

- Check docker container running and stop
	- docker container ls -a

- Start docker container had stopped yet
	- docker container start `<docker-name>`
	- vd: docker container start demo-backend

8. Lợi ích của Dockerfile - Inrastructure as Code + Immutable Infrastructure

- Inrastructure as Code
	- Quản lý run-time enviroment bằng code - Dockerfile
	- Đóng gói Application code cùng với run-time enviroment + configuration trong 1 object là Docker image. Sau đó sử dụng docker image để deploy lên các server. Lúc này docker image không thay đổi nên application sẽ chạy ổn định trên các môi trường
-  Immutable Infrastructure
	- Đóng gọi trạng thái cuối cùng sau khi cài đặt run-time enviroment và configuration và application code và tạo thành image để deploy lên khác môi trường khác => Infrastructure sẽ không thay đổi

=> Đây là 2 khái niệm quan trọng giúp application chạy ổn định giữa các môi trường 

9. docker-compose.yaml

định nghĩa các thành phần: `<Version>, <Services>, <Networks>, <Volumes>`
- version: '3'
- services
	- service-name là alias mà docker-compose tự động networking giữa các service với nhau. Sau khi mapping thì có thể sử dụng tên service làm host name của container chạy service đấy
	- volumes
	- ports: 8080: 80 - Forwards the exposed port 80 của container sang port 8080 trên host machine.
	- depends_on: định nghĩa service chạy trước khi chạy service hiện tại
	- restart: định nghĩa restart hay không khi gặp lỗi 
	- network

10. docker-compose comands

- docker-compose up : run docker ở chế độ dettack-mode - Ctrl+C để tắt
- docker-compose up -d : run docker ở chế độ background
- docker-compose down
- docker container ps
- docker-compose ps
- docker-compose config
- docker-compose config -q
- docker volume ls
- docker inspect value `<volume-name>`
- docker-compose top
- docker-compose log
- docker container exec iit `<container ID><shell container>`
- docker-compose rm
- docker logs -f `<docker-name>`

11. Deploy container trên production - Container Orchestration
- Container Orchestration là gì ?
	- Giống như nhạc trưởng trong 1 dàn nhạc giao hưởng thì đối với 1 application sẽ bao gồm nhiều service con để hoạt động thì khi quản lí các thành phần con hay các service thì cần tới Orchestration
- Feature:
	- Container Deployment: Deploy container sau đó chạy và dựng container trên mỗi cluster node
	- Schedule: Lên lịch để sử lí các khối lượng công việc hay để xử lí các khối lượng công việc work load trên mỗi cluster node
	- Scaling: Thay đổi số lượng container tùy thuộc vào số lượng công việc (work load) 
	- Networking: Liên kết các service
	- Service Discovery: Tìm kiếm các service ở trong cùng application
	- Resource Allocation: Phân bổ khố lượng tài nguyên vào container
	- Heal Monitoring: Kiểm soát trạng thái hoạt động or up-time trong container
- Lợi ích khi sử dụng container Orchestration:
	- High Availability: Tăng cao tính khả dụng của toàn cluster
	- Tối ưu hóa Computing Resurces của toàn cluster - vd: tối ưu hóa hiệu suất cpu
	- Tự động hóa Deployment

12. Các công cụ Container Orchestration phổ biến
- Docker Swarm 
	- nó được cài đặt kèm docker và có cách sử tương tự docker-compose
	- sử dụng cho application quy mô nhỏ hoặc vừa
- Apache Mesos
	- là công cụ quản lí cluster có thể sử dụng với nhiều mục đích khác nhau. Ta có thể sử dụng Apache Mesos để thao tác quản lí với vitual machine cũng như container
	- Các thành phần chính:
		- Master: quản lí các Agent chạy ở các cluster node
			- khi chạy Apache Mesos ở chế độ high availability sẽ có nhiều master. Lúc này Apache Mesos sẻ dùng ZooKeeper để quản lí các master. Nhiệm vụ của Zookeeper là chọn ra 1 master chạy ở chế độ active còn các master khạc chạy ở chế độ stand-by (chế độ backup). Khi active master chết thì Zookeeper sẽ lựa chọn ra 1 stand-by master thành active master để duy trì hoạt động của toàn hệ thống
		- Agents: sẽ được cài đặt ở toàn bộ cluster node. Nhiệm vụ là cũng cấp các thông tin về resource vd Cpu, Ram tới master 
		- Frameworks: chạy trên mesos.
			- Schedule: nhiệm vụ truy vấn tới mater thông tin về các resource có thể sử dụng và sau khi đã lựa chọn xong cái resource mà framework sẽ sử dụng thì sẽ gửi nội dung task của framework đấy tới master
			- Excuter: sẽ chạy các task do framework cung cấp
	- Khi sử dụng Apache Mesos thì phải kèm nhiều framworks khác
		- Apache Aurora để quản lí các service chạy liên tục cũng như cron-tab
		- Chronos là framwork để chạy và quản lí các cron-job thay thế cho cron-tab
		- Mesosphere marathon là 1 công cụ container orchestration để quản lí các container trên mesos
		- Có thể chạy Kubernetes trên Apache Mesos
- Kubernetes (k8s)
	- Thành phần chính của Kubernetes cluster
		- Control Plane (master node), 
			- api để giao tiếp với các worker node
			- etcd: để lưu trữ dữ liệu cũng như config của cluster
			- schedule để lên lịch và chạy các work load trên các cluster node hay work load
			...
		- Worker node
			- Kubelet là thành phần quản lí các container chạy trên worker node
			- Kube-proxy là thành phần quản lí networking giữa worker node và master

Reference:
 - [ ] [**FullStacKAGE-Go Pro Docker**](https://youtube.com/playlist?list=PL28xQzrHZLIUMesZIulyOY0UEbUJhaQd6)

 - [ ] [**Docker ghi chep-hocchudong**](https://github.com/hocchudong/ghichep-docker)

 - [ ] [**Docker ghi chep-hungviet99**](https://github.com/hungviet99/ghichep_docker)

 - [ ] [**Docker ghi chep-readthedocs**](https://docker-ghichep.readthedocs.io/en/latest/README/)

 - [ ] [**Microservices-node-react**](https://github.com/chesterheng/microservices-node-react/blob/master/section-03.md)

<!-- ==================================================================================================================== -->

## NGINX

Về cơ bản, NGINX cũng hoạt động tương tự như các web server khác. Khi bạn mở một trang web, trình duyệt của bạn sẽ liên hệ với server chứa website đó. Server sẽ tìm kiếm đúng file yêu cầu của website và gửi về cho bạn. Đây là một trình tự xử lý dữ liệu single – thread, nghĩa là các bước được thực hiện theo một trình tự duy nhất. Mỗi yêu cầu sẽ được tạo một thread riêng.
- Tuy nhiên, NGINX hoạt động theo kiến trúc bất đồng bộ (asynchronous) hướng sự kiện (event driven). Nó cho phép các threads tương đồng được quản lý trong một tiến process. Mỗi process hoạt động sẽ bao gồm các thực thể nhỏ hơn, gọi là worker connections dùng để xử lý tất cả threads.
- Worker connections sẽ gửi các yêu cầu cho worker process, worker process sẽ gửi nó tới master process, và master process sẽ trả lời các yêu cầu đó. Đó là lý do vì sao một worker connection có thể xử lý đến 1024 yêu cầu tương tự nhau. Nhờ vậy, NGINX có thể xử lý hàng ngàn yêu cầu khác nhau cùng một lúc.

0. access_log & error_log

Đây là những tệp tin mà NGINX sẽ sử dụng để log bất kỳ lỗi và số lần truy cập. Các bản ghi này thường được sử dụng để gỡ lỗi hoặc sửa chữa.

1. worker_process

Xác định có bao nhiêu cores của CPU làm việc với Nginx. Nginx sẽ sử dụng một CPU để xử lý các tác vụ của mình. Tùy theo mức độ hoạt động của web server mà chúng ta có thể thay đổi lại thiết lập này.

2. events

NGINX sử dụng mô hình xử lý kết nối dựa trên sự kiện(event) nên các directive được định nghĩa trong context này sẽ ảnh hưởng đến connection processing được chỉ định. Ví dụ ở trên là cấu hình số worker connection mà mỗi worker process có thể xử lý được.

3. worker_connections  

cho biết số lượng connection mà mỗi worker_process có thể xử lý. Mặc định, số lượng connection này được thiết lập là 1024

4. http 

Khi cấu hình Nginx như một web server hoặc reverse proxy, http context sẽ giữ phần lớn cấu hình. Context này sẽ chứa tất cả các directive và những context(block directive) cần thiết khác để xác định cách chương trình sẽ xử lý các kết nối HTTP và HTTPS.

5. include

Chỉ thị include (include /etc/nginx/mime.types) của nginx có vai trò trong việc thêm nội dung từ một file khác vào trong cấu hình nginx. Điều này có nghĩa là bất cứ điều gì được viết trong tập tin mime.types sẽ được hiểu là nó được viết bên trong khối http {}. Điều này cho phép bạn bao gồm một số lượng dài của các chỉ thị trong khối http {} mà không gây lộn xộn lên các tập tin cấu hình chính. Và nó giúp tránh quá nhiều dòng mã cho mục đích dễ đọc

6. default_type

Định nghĩa kiểu MIME mặc định. Khi Nginx dc yêu cầu một file, định dạng file dc khớp với kiểu dc khai báo trong lock types để trả về kiểu MIME phù hợp trong trường Content-type trong HTTP response header. Nếu định dạng file không khớp với bất kì kiểu MIME nào, thì giá trị mặc định của chỉ thị default_type sẽ được sử dụng.

7. resolver

- Xác định tên verver (name server) dc sử dụng để Nginx chuyển hostname thành địa chỉ IP và ngược lại. (resulve: # use local DNS)
- Chỉ rõ các máy chủ phân giải tên miền (DNS) được sử dụng bởi Nginx để phân giải hostname của các địa chỉ IP và ngược lại. Các kết quả truy vấn DNS được lưu trong bộ nhớ cache trong 1 thời gian, hoặc được chỉ rõ bởi giá trị TTL (Time-to-live) của máy chủ DNS, hoặc được chỉ rõ bởi giá trị thời gian cho đối số hợp lệ

8. upstream backend

- định nghĩa một cụm mà bạn có thể yêu cầu proxy. Nó thường được sử dụng để xác định cụm máy chủ web để cân bằng tải hoặc cụm máy chủ ứng dụng để định tuyến / cân bằng tải.
- Ở đây backend1 và backend2 chính là server name của 2 máy chủ web, ta có thể thay bằng địa chỉ IP tương ứng.
- Để truyền các request từ người dùng vào một group các server, tên của group được truyền vào với directive proxy_pass (hoặc fastcgi_pass, memcached_pass, uwsgi_pass, scgi_pass tùy thuộc vào giao thức). Trong bài viết này, virtual server chạy trên NGINX sẽ truyền tất cả các request tới backend upstream server

8.1. upstream

Ngữ cảnh upstream định nghĩa một pool của các server cái NGINX sẽ ủy quyền các request tới. Sau khi chúng ta tạo một khối upstream và định nghĩa một server bên trong nó chúng có thể tham chiếu nó bằng tên bên trong các khối location. Thêm nữa, một ngữ cảnh upstream có thể có nhiều server được gán trong nó vì rằng NGINX sẽ làm một vài load balancing khi ủy quyền các request.

8.2. Thuật toán cân bằng tải round-robin, least_conn, least_time, ip_hash, ...

- round-robin: Các request lần lượt được đẩy về 2 server backend1 và backend2 theo tỉ lệ dựa trên server weights, ở đây là 1:1

8.3 Bảo toàn session người dùng

Hãy thử tưởng tượng bạn có một ứng dụng yêu cầu đăng nhập, nếu khi đăng nhập, session lưu trên Backend 1, sau một hồi request lại được chuyển tới Backend 2, trạng thái đăng nhập bị mất, hẳn là người dùng sẽ vô cùng nản. 
- NGINX PLUS có cung cấp sticky directive, giúp NGINX tracks user sessions và đưa họ tới đúng upstream server.
- Dùng ip_hash làm phương thức cân bằng tải
	- Hash được sinh từ 3 chỉ số đầu của một IP, do đó tất cả IP trong cùng C-class network sẽ đc điều hướng tới cùng một backend.
	- Tất cả user phía sau một NAT sẽ truy cập vào cùng một backend.
	- Nếu ta thêm mới một backend, toàn bộ hash sẽ thay đổi, đương nhiên session sẽ mất.

9.  sendfile

cho phép send file.
giá trị mặc định sendfile: off

10. keepalive_timeout

- Vì các kết nối dạng keepalive được mở trong thời gian nhất định, những kết nối này cũng tốn một lượng tài nguyên. Vì thế ta cần cấu hình thời gian timeout của các kết nối này một cách hợp lý dựa trên ứng dụng/website và traffic của ứng dụng. Tùy theo cấu hình mà có thể tăng/giảm hiệu năng của ứng dụng khi gặp tải cao.
- keepalive_timeout time1[time2]
giá trị mặc định : 75. Tham số thứ 2 được đưa vào giá trị Keep-alive của HTTP response header để báo cho trình duyệt biết tự đóng kết nối trước khoảng thời gian time out do server chỉ định

11. client_max_body_size

Nó là kích thước tối đa của dữ liệu yêu cầu từ client. Nếu kích thước này bị vượt qua, Nginx trả về 1 lỗi HTTP 413 Request entity too large. Thiết lập này đặc biệt quan trọng nếu chúng ta cho phép người dùng tải các tập tin lên máy chủ qua HTTP

12. server

Ngữ cảnh server định nghĩa một server ảo để xử lý các request từ client của bạn. Bạn có thể có nhiều khối server, và NGINX sẽ chọn một trong số chúng dựa trên các chỉ thị listen và server_name.
- Trong một khối server, chúng ta định nghĩa nhiều ngữ cảnh location được sử dụng để quyết định cách xử lý các request từ client. Bất cứ khi nào một request đến, NGINX sẽ thử khớp URI tới một trong số các định nghĩa location và xử lý nó cho phù hợp

13. listen

- Chỉ rõ địa chỉ IP và/hoặc port được dùng bởi socket phục vụ website. Các website thường được phục vụ trên port 80 (giá trị mặc định) qua HTTP, hoặc 443 qua HTTPS.
- Cú pháp: listen [address] [:port] [additional options];
- dditional options: 
+ ssl: Chỉ rõ website sẽ sử dụng SSL.

14. server_name 

- Đăng ký 1 hoặc nhiều hostname cho khối server. Khi Nginx nhận 1 yêu cầu HTTP, nó so sánh giá trị Host trong phần header của yêu cầu với tất cả các khối server đang có. Khối server đầu tiên khớp với hostname này sẽ được chọn.
- Nếu không có khối server nào khớp với hostname trên, Nginx chọn khối server đầu tiên khớp với các thông số của chỉ thị listen (ví dụ như listen *:80 sẽ bắt tất cả các yêu cầu nhận được trên port 80), ưu tiên khối đầu tiên có tùy chọn mặc định được cho phép trên chỉ thị listen.

15. ssl

SSL là viết tắt của từ Secure Sockets Layer. Đây là một tiêu chuẩn an ninh công nghệ toàn cầu tạo ra một liên kết được mã hóa giữa máy chủ web và trình duyệt. Liên kết này đảm bảo tất cả các dữ liệu trao đổi giữa máy chủ web và trình duyệt luôn được bảo mật và an toàn. 

SSL đảm bảo rằng tất cả các dữ liệu được truyền giữa các máy chủ web và các trình duyệt được mang tính riêng tư, tách rời. SSL là một chuẩn công nghiệp được sử dụng bởi hàng triệu trang web trong việc bảo vệ các giao dịch trực tuyến với khách hàng của họ.
HTTP -> HTTPS


16. location

- Sau khi đã chọn được server block nào sẽ tiếp nhận request này thì nginx sẽ tiếp tục phân tích URI của request để tìm ra hướng xử lí của request dựa vào các block location có syntax như sau:
location optional_modifier location_match {
    . . .
}
- optional_modifier: bạn có thể tạm hiểu nó là kiểu so sánh để tìm ra để đối chiếu với location_match. Có mấy loại option như sau:
	- (none): Nếu không khai báo gì thì NGINX sẽ hiểu là tất cả các request có URI bắt đầu bằng phần location_match sẽ được chuyển cho location block này xử lí.
	- = : Khai báo này chỉ ra rằng URI phải có chính xác giống như location_match (giống như so sánh string bình thường).
	- ~ : Sử dụng regular expression cho các URI
	- ~* : Sử dụng regular expression cho các URI cho phép pass cả chữ hoa và chữ thường

16.1. index directive

index direct nằm bên trong location luôn được nginx trỏ tới đầu tiên khi xử lí điều hướng request. Định nghĩa trang mặc định mà Nginx sẽ phục vụ nếu không có tên tập tin được chỉ rõ trong yêu cầu (nói cách khác, trang chỉ mục). Chúng ta có thể chỉ rõ nhiều tên tập tin và tập tin đầu tiên được tìm thấy sẽ được sử dụng. Nếu không có tập tin cụ thể nào được tìm thấy, Nginx sẽ hoặc là cố gắng phát sinh 1 chỉ mục tự động của các tập tin
location = / {
    root html;
    index index.html;
}
- Nginx sẽ đọc root directive để xác định thư mục chứa trang client yêu cầu. Thứ tự các trang được ưu tiên sẽ được khai báo trong index directive.
Nếu không tìm được nội dung mà client yêu cầu, nginx sẽ điều hướng sang location context khác và thông báo lỗi cho người dùng.


16.2  error_page directive

chỉ thị khi không tìm thấy file tham chiếu.
location / {
    error_page 404 = @fallback;
}

location @fallback {
    proxy_pass http://backend;
}

17. Có nhiều chỉ thị quan trọng có thể được sử dụng dưới ngữ cảnh location

- try_files sẽ cố gắng phục vụ các tệp tin tĩnh được tìm thấy trong thư mục được trỏ tới bởi chỉ thị gốc.
- proxy_pass sẽ gửi request tới một proxy server cụ thể.
- rewrite sẽ viết lại URI tới dựa trên một regular expression để một khối location có thể xử lý nó.

I. INSTALL by WSL

1. Open windowns powershell and enter:

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

2. Search on your windows:

Turn Windows features on or off

3. Enable: Windows Subsystem for Linux

4. Enable: Vitual machine platforms

5. Download linux (wsl) on Microsoft store

I choise Ubuntu18.04

6. Create UNIX account

7. Open wsl or open cmd or windowns powershell:
wsl

sudo apt-get update

sudo apt-get upgrade

sudo add-apt-repository ppa:nginx/stable

sudo apt-get update

sudo apt-get install -y nginx

note: Your nginx will install at C:\Users\`<Username>`\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs\etc\nginx

8. Start nginx by:

sudo service nginx start 

nginx will start default at: localhost:80

note: Some command nginx for wsl:

+ sudo systemctl start nginx 
sudo systemctl stop nginx 
sudo systemctl restart nginx

+ sudo service nginx start
sudo service nginx stop
sudo service nginx restart

+ sudo /etc/init.d/nginx start
sudo /etc/init.d/nginx stop
sudo /etc/init.d/nginx restart

9. Let's play

10. Unistall nginx by wsl

- Removes all but config files: 
sudo apt-get remove nginx nginx-common

- Removes everything:
sudo apt-get purge nginx nginx-common

- After using any of the above commands, use this in order to remove dependencies used by nginx which are no longer required:
sudo apt-get autoremove


II. Nginx for windowns

1. Download at: http://nginx.org/en/download.html

choose nginx/windowns-`<version>`

2. Unpack and use nginx by the ways double click nginx.exe

3. Some example for the drive C: root directory:

cd c:\
unzip nginx-1.17.9.zip
cd nginx-1.17.9
start nginx

- Run the tasklist command-line utility to see nginx processes:
tasklist /fi "imagename eq nginx.exe"

<!-- Image Name           PID Session Name     Session#    Mem Usage
=============== ======== ============== ========== ============
nginx.exe            652 Console                 0      2 780 K
nginx.exe           1332 Console                 0      3 112 K -->

| Image Name   |      PID      |  Session Name |  Session# |  Mem Usage |
|----------|:-------------:|------:|------:|------:|
| nginx.exe |  652 | Console | 0 | 2 780 K |
| nginx.exe |  1332 | Console | 0 | 3 112 K |

- If nginx does not start, look for the reason in the error log file:
logs\error.log

- If an error page is displayed instead of the expected page, also look for the reason in the logs\error.log file.

- nginx/Windows runs as a standard console application (not a service), and it can be managed using the following commands:
+ nginx -s stop 		fast shutdown
+ nginx -s quit	    	graceful shutdown
+ nginx -s reload		changing configuration, starting new worker processes with a new configuration, graceful shutdown of old worker processes
+ nginx -s reopen		re-opening log files

4. Let's play


III. Nginx in docker

-- Updating --

<!-- ==================================================================================================================== -->

## PM2

Tự động chạy ứng dụng Nodejs, tự động restart khi lỗi. PM2 còn kèm theo nhiều plugin với nhiều chức năng như: cân bằng tải, update source no zero down time (cập nhật mã nguồn mà không làm tắt server), scale (tự động mở rộng khả năng chịu tải), tự động cập nhật source code khi Git được update, …

- Quản lý các process, bao gồm tự động restart app khi bị chết hoặc reboot hệ thống.
- Giám sát ứng dụng
- Khai báo cấu hình qua JSON file
- Quản lý log
- Cluster mode
- Chạy các kịch bản lệnh cho hệ thống
- Seamless updates
- Cho phép tích hợp các module cho hệ thống


// Khởi động lại ứng dụng thông qua app name

`pm2 restart <app_name>`
 
// Tải lại lại ứng dụng thông qua app name

`pm2 reload <app_name>`
 
// Dừng ứng dụng thông qua app name

`pm2 stop <app_name>`
 
// Xóa ứng dụng thông qua app name

`pm2 delete <app_name>`
 
// Khởi động lại ứng dụng thông qua app name

`pm2 restart <app_name>`
 
// Tải lại lại ứng dụng thông qua app name

`pm2 reload <app_name>`
 
// Dừng ứng dụng thông qua app name

`pm2 stop <app_name>`
 
// Xóa tất cả ứng dụng đang chạy của Pm2

`pm2 kill`

// Xem nhật ký (logs) trong PM2

`pm2 logs`




