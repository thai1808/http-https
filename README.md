# http-https
HTTP VÀ HTTPS
1. HTTP
- HTTP hoạt động theo mô hình Client– Server. 
- Khi truy cập một trang web qua giao thức HTTP, trình duyệt sẽ thực hiện các phiên kết nối đến server của trang web đó thông qua địa chỉ IP do hệ thống phân giải tên miền DNS cung cấp. 
- Server khi nhận được request, sẽ trả về lệnh tương ứng giúp hiển thị website, bao gồm các nội dung như: văn bản, ảnh, video, âm thanh,…
- Dữ liệu được gửi đi qua giao thức HTTP (IP, username, password nhập vào trang web để login) cũng không hề được mã hóa và bảo mật. 

-> Dễ bị lộ thông tin khi bị nghe lén (VD bắt gói tin bằng Wireshark)

 <img width="387" alt="Screenshot_1" src="https://user-images.githubusercontent.com/68786130/104394101-82926200-5578-11eb-94da-390801fe4652.png">

- Là giao thức không lưu trạng thái stateless protocol (server không lưu bất cứ dữ liệu nào giữa các yêu cầu)
 Các thành phần chính của HTTP
a. HTTP Requests
- HTTP Request Method: là phương thức để chỉ ra hành động mong muốn được thực hiện trên tài nguyên đã xác định

 
 Request sử dụng method GET: được sử dụng để lấy lại thông tin từ Server một tài nguyên xác định
Request Version là HTTP 1.1
Host là kenh14.vn và 1 số thông tin đi kèm được Wireshark bắt được trong gói tin như Language, Encoding, Connection,…
- Bên cạnh GET còn có các method khác như POST, PUT, DELETE,…
+ POST: yêu cầu server chấp nhận thực thể được đính kèm trong Request được xác định bởi URI, ví dụ, thông tin khách hàng, file upload,…
+ PUT: Nếu URI đề cập đến 1 tài nguyên đã có, nó  sẽ bị sửa đổi. Nếu URI không trỏ đến 1 tài nguyên hiện có, thì server có thể tạo ra tài nguyên với URI hiện có
b. HTTP Reponses

 
 Server trả về cho Client với Status Code: 301 (tự động chuyển hướng user đến vị trí mới của file – redirect)
 
 Hay với trang web khác hiển thị Status Code là 200 OK – đây là phản hồi tiêu chuẩn cho các yêu cầu HTTP thành công


c. HTTP Caching
- Trong các ứng dụng Web hiện nay, đều có cache lại kết quả của các câu truy vấn để trả về kết quả nhanh hơn
- HTTP Caching là việc chuyển 1 bản coppy các tài nguyên tĩnh phía Server xuống lưu ở dưới Client  giúp ứng dụng Web load nhanh hơn (giảm thời gian trễ), giảm băng thông sử dụng, giảm số lần truy cập lên Server
 
 Expires cho biết Cache có thời hạn lưu trữ là bao lâu. Sau thời điểm này nếu Client vẫn muốn sử dụng file thì phải lên Server để download lại cũng như cập nhật thông tin Expires mới, còn nếu chưa hết hạn thì chỉ việc lấy thông tin từ cache để sử dụng mà không cần hỏi lại Server
Last-Modified: Server có thể bao gồm Last-Modified header cho biết ngày và thời gian mà một số content được sửa đổi lần cuối
Cache-Control xác định thời gian và nội dung được lưu trong cache như thế nào
private: content sẽ không được lưu trong bất kỳ proxies nào và nó sẽ chỉ được cached ở client
public: ngoài việc lưu cache ở client, còn có thể được cache bởi proxies, phục vụ cho các user khác
max-age: seconds xác định số giây cho mỗi content sẽ được cache
no-store: xác định content sẽ không được lưu vào cache.
no-cache: cache có thể được duy trì nhưng content được lưu trong cache sẽ được validate lại từ server
must-revalidate: thỉnh thoảng xảy ra khi gặp vấn đề về network và content không thể lấy về từ server, browser có thể phục vụ content cũ mà không cần validation.
d. HTTP Cookies
- Cookie là một đoạn văn bản mà một Web server có thể lưu trên ổ cứng của user. 
- Cookie cho phép một website lưu các thông tin trên máy tính của người dùng và sau đó lấy lại nó. Các mẩu thông tin sẽ được lưu dưới dạng cặp tên – giá trị (name-value)
 
- Cookie được chia làm 2 loại:
+ Session Cookie: Được lưu tạm thời trong bộ nhớ máy tính trong lúc user đang truy cập Website đó và sẽ tự động xóa khi đóng trình duyệt
VD: Tắt laptop đột ngột và khi mở lên, trình duyệt sẽ tự động hỏi user có muốn load lại những trang web vừa truy cập không  Session Cookie
+ Persistant Cookie: lưu trên ổ cứng máy tính và không bị xóa khi đóng trình duyệt, được dùng để cung cấp những hành vi, hành động của bạn khi truy cập Website đó
 
 Hai trường Secure hay HttpOnly đều không có giá trị xác định, được cho vào cookie để xác nhận cookie có sử dụng nó hay không.
Secure có tác dụng làm cho trình duyệt phải sử dụng kết nối secure/encrypted - kết nối bảo mật, được mã hóa. Hoạt động khi server có sử dụng SSL. 
HttpOnly có tác dụng làm cho cookie chỉ được thao tác bởi server mà không bị thao tác bởi các script phía người dùng.
e. CORS – Cross-Origin Resource Sharing
Trước hiểu cần hiểu về SOP (Same-origin Policy)
- Là cơ chế bảo mật quan trọng trong trình duyệt
- Hạn chế Document hoặc script được tải từ một nguồn có thể tương tác với tài nguyên từ nguồn khác *Mục đích: Bảo vệ người dùng khi truy cập trang web độc hại
 Origin:  Host, Protocol, Port
Ví dụ: Cho URL: http://store.company.com/dir/page.html

URL	Kết quả	Nguyên nhân
http://store.company.com/dir2/other.html	Thành công	Cùng giao thức và host
http://store.company.com/dir/inner/another.html	Thành công	Cùng giao thức và host
https://store.company.com/secure.html	Thất bại	Khác giao thức
http://store.company.com:8080/secure.html	Thất bại (*)	Cùng giao thức và host nhưng khác port
http://shop.company.com/secure.html	Thất bại	Khác host


- Nếu truy cập trang web có mã độc và trang web đó sử dụng Javascript để truy cập tin nhắn Facebook ở địa chỉ https://facebook.com/messages
- Giả sử đã đăng nhập Facebook từ trước, nếu không có SOP, trang web độc hại kia có thể thoải mái lấy dữ liệu và bất cứ điều gì chúng muốn
 Tuy nhiên để có thể truy cập được đến domain khác cần dùng CORS 
- CORS sử dụng các HTTP header để “thông báo” cho trình duyệt rằng, một ứng dụng web chạy ở origin này (thường là domain này) có thể truy cập được các tài nguyên ở origin khác (domain khác).
- Một ứng dụng web sẽ thực hiện truy vấn HTTP cross-origin nếu nó yêu cầu đến các tài nguyên ở origin khác với origin nó đang chạy (khác giao thức, domain, port).
- VD: Một ứng dụng web chạy ở domain aaa.com và nó cần truy vấn đến bbb.com để lấy một vài dữ liệu (thường được thực hiện bởi JavaScript bằng cách sử dụng XMLHttpRequest).
 Các trình duyệt đều cài đặt SOP và tuân thủ nó rất chặt chẽ. Cài đặt XMLHttpRequest và kể cả Fetch API cũng đều tuân thủ chính sách này. Do đó những truy vấn như ở trên sẽ không thu được kết quả gì, trừ khi máy chủ trả về response có các header CORS phù hợp.
 Sử dụng CORS sẽ thúc đẩy việc giao tiếp trong ứng dụng web dễ dàng hơn
 Bắt gói tin HTTP
 
 Truy cập trang web http://jeanshop.vn và đăng nhập 
 
 Dữ liệu vừa nhập đã được Wireshark bắt được  Có thể đọc được Username và Password ở dạng Plaintext
 
 Tương tự với http://45.252.248.114 cũng hiện thị username và password dạng Plaintext



2. HTTPS
- HTTPS viết tắt của Hyper Text Transfer Protocol Secure (giao thức truyền tải siêu văn bản bảo mật) là phiên bản an toàn của HTTP. 
- Chữ 'S' ở cuối HTTPS là viết tắt của "Secure" (Bảo mật)  tất cả các giao tiếp giữa trình duyệt và trang web đều được mã hóa.
 
- HTTPS thường sử dụng một trong hai giao thức bảo mật để mã hóa thông tin liên lạc - SSL (Secure Sockets Layer) hoặc TLS (Transport Layer Security). 
- Cả hai giao thức TLS và SSL đều sử dụng hệ thống PKI (Public Key Infrastructure - hạ tầng khóa công khai) không đối xứng. 
- Sử dụng hai khóa để mã hóa thông tin liên lạc là Public Key và Private Key. 
- Bất cứ thứ gì được mã hóa bằng Public Key chỉ có thể được giải mã bởi Private Key và ngược lại.
 Lý do sử dụng HTTPS
- HTTPS bảo mật tốt thông tin người dùng: các thông tin trao đổi giữa browser và server sẽ không bị nghe lén trên đường truyền nhờ phương thức mã hóa.
*Nếu dùng HTTP có thể bị sniffing và attacker có thể lấy toàn bộ dữ liệu từ email, thẻ tín dụng, password…cùng các thông tin sẵn có trên web. Hoặc những thao tác trên website của người dùng có thể bị ghi lại mà không hề hay biết.
- Tránh bị lừa đảo bằng website giả mạo
- Tăng uy tín với người dùng
- Các browser hiện này đều có những cảnh báo đến người dùng về những trang web không được bảo mật – không có ổ khóa xanh 
