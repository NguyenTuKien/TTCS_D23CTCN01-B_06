# Phần 1: Message Queue là gì? 
Message Queue (MQ) là một hệ thống trung gian cho phép các ứng dụng giao tiếp với nhau thông qua việc gửi và nhận các tin nhắn. MQ giúp tách rời các thành phần của hệ thống, cho phép chúng hoạt động độc lập và không bị ảnh hưởng bởi nhau. Điều này giúp tăng tính linh hoạt, khả năng mở rộng và độ tin cậy của hệ thống.

**Tại sao sử dụng Message Queue?**
- **Tách rời các thành phần**: MQ cho phép các dịch vụ hoạt động độc lập, giúp giảm sự phụ thuộc giữa chúng.
- **Xử lý bất đồng bộ**: Các tin nhắn có thể được gửi và nhận mà không cần phải chờ đợi, giúp cải thiện hiệu suất hệ thống.
- **Tăng khả năng mở rộng**: MQ cho phép thêm hoặc bớt các thành phần một cách dễ dàng mà không ảnh hưởng đến toàn bộ hệ thống.
- **Đảm bảo độ tin cậy**: MQ có thể lưu trữ các tin nhắn cho đến khi chúng được xử lý, giúp đảm bảo rằng không có dữ liệu nào bị mất.

**Lợi ích của việc sử dụng Message Queue**
- **Cải thiện hiệu suất**: Bằng cách xử lý các tác vụ một cách bất đồng bộ, hệ thống có thể hoạt động nhanh hơn và hiệu quả hơn.
- **Tăng tính sẵn sàng**: Nếu một thành phần gặp sự cố, các tin nhắn vẫn có thể được lưu trữ và xử lý sau khi thành phần đó hoạt động trở lại.  
- **Dễ dàng mở rộng**: Hệ thống có thể dễ dàng mở rộng bằng cách thêm nhiều hàng đợi hoặc các dịch vụ xử lý tin nhắn mới.
- **Quản lý tải**: MQ giúp phân phối tải công việc đều hơn giữa các thành phần, tránh tình trạng quá tải.  
- **Hỗ trợ đa nền tảng**: MQ thường hỗ trợ nhiều ngôn ngữ lập trình và nền tảng khác nhau, giúp tích hợp dễ dàng hơn.

**Nhược điểm của việc sử dụng Message Queue**
- **Độ trễ**: Việc gửi và nhận tin nhắn qua MQ có thể gây ra độ trễ so với giao tiếp trực tiếp.
- **Phức tạp trong quản lý**: Việc triển khai và quản lý hệ thống MQ có thể phức tạp và đòi hỏi kiến thức chuyên sâu.
- **Chi phí vận hành**: Sử dụng MQ có thể tăng chi phí vận hành do cần phải duy trì hệ thống trung gian.
- **Khó khăn trong việc debug**: Việc theo dõi và gỡ lỗi các vấn đề liên quan đến MQ có thể khó khăn hơn so với các hệ thống trực tiếp.
- **Yêu cầu bảo mật**: Việc truyền tin nhắn qua MQ có thể tạo ra các lỗ hổng bảo mật nếu không được quản lý đúng cách.

Vậy nên, việc sử dụng Message Queue cần được cân nhắc kỹ lưỡng dựa trên yêu cầu và đặc điểm của hệ thống để tận dụng tối đa lợi ích mà nó mang lại.

---
# Phần 2: Các hệ thống Message Queue phổ biến
Dưới đây là một số hệ thống Message Queue phổ biến được sử dụng rộng rãi trong ngành công nghiệp phần mềm:

## Kafka

**Apache Kafka** là một nền tảng **distributed event streaming** (xử lý luồng sự kiện phân tán) mã nguồn mở. Kafka được thiết kế để **xử lý throughput rất lớn**, **độ trễ thấp**, **mở rộng ngang tốt** và lưu trữ dữ liệu dạng log theo thời gian (retention). Kafka thường dùng cho các bài toán: thu thập log, event-driven microservices, streaming dữ liệu realtime, analytics, pipeline dữ liệu.

### 1. Một số khái niệm cơ bản trong Kafka

* **Producer:** Ứng dụng gửi (publish) message/event vào Kafka.
* **Consumer:** Ứng dụng đọc (subscribe) message/event từ Kafka.
* **Topic:** “Chủ đề” để phân loại message. Producer gửi vào topic, consumer đọc từ topic.
* **Partition:** Mỗi topic được chia thành nhiều partition. Partition là **đơn vị song song (parallelism)** và **thứ tự (ordering)** trong Kafka.

  * **Kafka chỉ đảm bảo thứ tự trong cùng 1 partition**, không đảm bảo thứ tự toàn topic nếu có nhiều partition.
* **Broker:** Một node Kafka server. Một cluster Kafka gồm nhiều broker.
* **Replication (bản sao):** Mỗi partition có thể có nhiều bản sao (replicas) trên các broker khác nhau để tăng độ tin cậy.
* **Leader / Follower (Replica):**

  * Mỗi partition có **1 leader** (nhận ghi/đọc chính)
  * Các follower replicate dữ liệu từ leader.
* **Offset:** Số thứ tự của message trong 1 partition. Consumer đọc message theo offset.
* **Consumer Group:** Nhóm consumer cùng đọc 1 topic để chia tải.
  Quy tắc quan trọng:

  * Trong **một consumer group**, **mỗi partition chỉ được gán cho tối đa 1 consumer** tại một thời điểm.
  * Muốn tăng song song → tăng số partition hoặc tăng số group phù hợp.
* **Retention:** Kafka lưu message theo thời gian hoặc dung lượng (ví dụ giữ 7 ngày), không phải “đọc xong là mất” như queue truyền thống.
* **Acknowledgement (acks):** Cơ chế producer chờ xác nhận ghi thành công:

  * `acks=0`: không chờ xác nhận (nhanh nhưng rủi ro mất)
  * `acks=1`: leader ack
  * `acks=all`: leader + ISR (an toàn hơn)

> Lưu ý: Kafka thường được gọi là “message queue”, nhưng bản chất mạnh nhất của nó là **commit log / event streaming**.

### 2. Cơ chế hoạt động (luồng dữ liệu)

1. **Producer** gửi event vào một **topic**.
2. Kafka quyết định ghi event vào **partition** nào (theo key hoặc round-robin).
3. Event được ghi append-only vào log của partition (tạo **offset**).
4. **Consumer** trong một **consumer group** sẽ được phân công (assign) các partition để đọc.
5. Consumer xử lý xong sẽ **commit offset** (để Kafka biết consumer đã đọc tới đâu).

### 3. Partitioning và Ordering (thứ tự)

* Kafka **đảm bảo thứ tự theo partition**, nên nếu bạn cần “events của cùng 1 user phải theo đúng thứ tự”, bạn nên:

  * gửi message với **key = userId**
    → Kafka sẽ route các message cùng key vào cùng partition → giữ đúng order cho user đó.

### 4. Delivery semantics (đảm bảo giao nhận)

Kafka thường gặp 3 khái niệm:

* **At most once:** Có thể mất message, không xử lý trùng.
* **At least once (phổ biến nhất):** Không mất message nhưng **có thể xử lý trùng** nếu retry / consumer crash → cần **idempotency** ở consumer.
* **Exactly once (EOS):** Kafka có cơ chế hỗ trợ “exactly-once” (phức tạp hơn), thường dùng khi streaming/pipeline cần độ chính xác cao.

### 5. Ưu điểm & nhược điểm

**Ưu điểm**

* Throughput rất lớn, scale ngang tốt (thêm broker/partition).
* Retention: lưu event theo thời gian → replay/đọc lại được.
* Consumer group giúp chia tải tốt.
* Phù hợp event-driven architecture, streaming, analytics.

**Nhược điểm**

* Vận hành/cấu hình phức tạp hơn (partition, replication, tuning).
* Không “dễ dùng” như RabbitMQ cho các job queue nhỏ.
* Không tối ưu cho routing linh hoạt kiểu exchange/binding như RabbitMQ.

---
## Rabbit MQ
### 1. Một số khái niệm cơ bản trong Rabbit MQ
* **Producer:** Ứng dụng gửi message.
* **Consumer:** Ứng dụng nhận message.
* **Queue:** Lưu trữ messages.
* **Message:** Thông tin truyền từ Producer đến Consumer qua RabbitMQ.
* **Connection:** Một kết nối TCP giữa ứng dụng và RabbitMQ broker.
* **Channel:** Một kết nối ảo trong một Connection. Việc publishing hoặc consuming từ một queue đều được thực hiện trên channel.
* **Exchange:** Là nơi nhận message được publish từ Producer và đẩy chúng vào queue dựa vào quy tắc của từng loại Exchange. Để nhận được message, queue phải được nằm trong ít nhất 1 Exchange.
* **Binding:** Đảm nhận nhiệm vụ liên kết giữa Exchange và Queue.
* **Routing key:** Một key mà Exchange dựa vào đó để quyết định cách để định tuyến message đến queue. Có thể hiểu nôm na, Routing key là địa chỉ dành cho message.
* **AMQP:** Giao thức Advance Message Queuing Protocol, là giao thức truyền message trong RabbitMQ.
* **User:** Để có thể truy cập vào RabbitMQ, chúng ta phải có username và password. Trong RabbitMQ, mỗi user được chỉ định với một quyền hạn nào đó. User có thể được phân quyền đặc biệt cho một Vhost nào đó.
* **Virtual host/Vhost:** Cung cấp những cách riêng biệt để các ứng dụng dùng chung một RabbitMQ instance. Những user khác nhau có thể có các quyền khác nhau đối với vhost khác nhau. Queue và Exchange có thể được tạo, vì vậy chúng chỉ tồn tại trong một vhost.

<div style="text-align:center;">
  <img src="https://images.viblo.asia/a1571d98-cb4e-4f3a-9757-117a492be32c.png" alt="Sơ đồ vận chuyển message trong RabbitMQ" />
</div>

### 2. Các loại Exchange trong Rabbit MQ
#### a. Direct Exchange
Direct Exchange vận chuyển message đến queue dựa vào routing key. Thường được sử dụng cho việc định tuyến tin nhắn unicast-đơn hướng (mặc dù nó có thể sử dụng cho định tuyến multicast-đa hướng). Các bước định tuyến message:
* Một queue được ràng buộc với một direct exchange bởi một routing key K.
* Khi có một message mới với routing key R đến direct exchange. Message sẽ được chuyển tới queue đó nếu R=K.

<div style="text-align:center;">
  <img src="https://images.viblo.asia/58a67bc4-e097-44a4-95d2-5d89a7e2e6f5.png" alt="Sơ đồ vận chuyển message trong RabbitMQ" />
</div>

#### b. Default Exchange
Mỗi một exchange đều được đặt một tên không trùng nhau, default exchange bản chất là một direct exchange nhưng không có tên (string rỗng). Nó có một thuộc tính đặc biệt làm cho nó rất hữu ích cho các ứng dụng đơn giản: mọi queue được tạo sẽ tự động được liên kết với nó bằng một routing key giống như tên queue.

Ví dụ, nếu bạn tạo ra 1 queue với tên "hello-world", RabbitMQ broker sẽ tự động binding default exchange đến queue "hello-word" với routing key "hello-world".

#### c. Fanout Exchange
Fanout exchange định tuyến message tới tất cả queue mà nó bao quanh, routing key bị bỏ qua. Giả sử, nếu nó N queue được bao quanh bởi một Fanout exchange, khi một message mới published, exchange sẽ vận chuyển message đó tới tất cả N queues. Fanout exchange được sử dụng cho định tuyến thông điệp broadcast - quảng bá.

<div style="text-align:center;">
  <img src="https://images.viblo.asia/e98bbff8-80e3-48fe-9288-5650ebf38bf4.png" alt="Sơ đồ vận chuyển message trong RabbitMQ" />
</div>

#### d. Topic Exchange
Topic exchange định tuyến message tới một hoặc nhiều queue dựa trên sự trùng khớp giữa routing key và pattern. Topic exchange thường sử dụng để thực hiện định tuyến thông điệp multicast. Ví dụ một vài trường hợp sử dụng:
* Phân phối dữ liệu liên quan đến vị trí địa lý cụ thể.
* Xử lý tác vụ nền được thực hiện bởi nhiều workers, mỗi công việc có khả năng xử lý các nhóm tác vụ cụ thể.
* Cập nhật tin tức liên quan đến phân loại hoặc gắn thẻ (ví dụ: chỉ dành cho một môn thể thao hoặc đội cụ thể).
* Điều phối các dịch vụ của các loại khác nhau trong cloud

#### e. Headers Exchange
Header exchange được thiết kế để định tuyến với nhiều thuộc tính, đễ dàng thực hiện dưới dạng tiêu đề của message hơn là routing key. Header exchange bỏ đi routing key mà thay vào đó định tuyến dựa trên header của message. Trường hợp này, broker cần một hoặc nhiều thông tin từ application developer, cụ thể là, nên quan tâm đến những tin nhắn với tiêu đề nào phù hợp hoặc tất cả chúng.

## So sánh RabbitMQ và Kafka

| Tiêu chí                           | RabbitMQ                       | Kafka                                       |
| ---------------------------------- | ------------------------------ | ------------------------------------------- |
| Dùng khi                           | Job/task queue (xử lý nền)     | Event streaming (log sự kiện)               |
| Throughput                         | Tốt                            | Rất cao                                     |
| Latency                            | Rất thấp (ms)                  | Thấp nhưng thiên về throughput              |
| Lưu message                        | Consume xong thường mất        | Lưu theo retention, đọc lại được            |
| Retry/DLQ                          | Dễ, built-in mạnh              | Làm được nhưng thường phải tự thiết kế      |
| Vận hành                           | Dễ hơn                         | Phức tạp hơn                                |
| Phù hợp dự án check-in + cộng điểm | ✅ Rất hợp (đơn giản, hiệu quả) | ✅ Hợp nếu muốn scale cực lớn + replay event |

