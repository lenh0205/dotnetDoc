
# Cross-Origin Resource Sharing (CORS)
```
No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed 
access
```
* Authentication Cookies được sử dụng để thông báo cho server biết rằng bạn đã đăng nhập vào hệ thống, và chúng được tự động gửi kèm mỗi request lên server
```
// ta đã đăng nhập vào Facebook đang sử dụng Authentication Cookies
// ta click 1 link bất kỳ trên mạng -> redirect bạn về 1 website nào đó 
// Website này sẽ tự động chạy 1 đoạn code Javascript để thực hiện request lên "facebook.com" có kèm theo authentication cookie của bạn
// Nếu không có CORS, hacker có thể thực hiện các thao tác trên Facebook với tài khoản của ta
// Ví dụ như đăng tin lên trên dòng thời gian của bạn kèm theo link, sau đó bạn bè ta lại click vào link 
// Nhưng với CORS,Facebook sẽ chỉ cho phép những request với Origin (đính kèm trong request header) là facebook.com lên server của họ
```
* nếu thay đổi header origin khi gửi request -> nhưng trình duyệt sẽ tự động bỏ qua và chỉ gửi lên origin thực sự
* nếu request được thực hiện từ phía server chứ không phải client -> có thể vượt qua CORS nhưng không thể gửi kèm được authentication cookie (nằm ở phía client)

# Content Security Policy (CSP)
* XSS - cross-site scripting, là khi kẻ xấu nhúng code Javascript vào trong code phía client của bạn
```
// Nếu nhúng được code Javascript vào website mà ta đang truy cập, thì có thể:
-> Giả dạng bạn để thực hiện một HTTP request.
-> Nhúng 1 iframe trông như 1 phần website và yêu cầu bạn nhập mật khẩu rồi gửi request đến server của hacker.
-> Chèn hoặc sửa một đường dẫn trên website gốc, dẫn đến một website giả mạo với giao diện giống hệt website gốc để thực hiện hành vi lừa đảo (ví dụ yêu cầu đăng nhập, yêu cầu nhập thông tin tài khoản, ...)
```
* CSP sẽ cố gắng ngăn chặn điều này ngay từ đầu bằng cách giới hạn: Cái gì có thể được phép mở trong một iframe, Style nào có thể được tải, Request có thể được thực hiện ở đâu, ...
```
VD: GET request lên facebook, reponse header như sau:
content-security-policy:
default-src * data: blob:;
script-src *.facebook.com *.fbcdn.net *.facebook.net *.google-analytics.com *.virtualearth.net *.google.com 127.0.0.1:* *.spotilocal.com:* 'unsafe-inline' 'unsafe-eval' *.atlassolutions.com blob: data: 'self';
style-src data: blob: 'unsafe-inline' *;
connect-src *.facebook.com facebook.com *.fbcdn.net *.facebook.net *.spotilocal.com:* wss://*.facebook.com:* https://fb.scanandcleanlocal.com:*;
```
* `default-src`: Hạn chế tất cả các CSP directive mà không được liệt kê rõ ràng.

* `script-src`: Giới hạn những script có thể được load.

* `style-src`: Giới hạn những style có thể được load.

* `connect-src`: Giới hạn những URL nào có thể được load sử dụng script như fetch, XHR, ajax, ...
* ngoài ra còn nhiều CSP directives khác

# Authentication
* bản chất là xác thực một HTTP request bằng một dấu hiệu nào đó
* authentication trong ứng dụng web: server xác thực `yêu cầu` do đúng là từ client của người dùng gửi lên ; server xác thực `nội dung yêu cầu có` đúng do 1 người dùng cụ thể gửi lên 

* HTTP request là một **`stateless protocol`** -> server xử lý các request một cách độc lập, không phụ thuộc vào trạng thái hay kết quả của request trước -> luôn phải xác minh user mỗi lần request

* Bản chất của `HTTP request` cũng là một bản tin biểu diễn bằng text -> sẽ cần một `dấu hiệu nào đó được thống nhất` để app biết nó xuất phát từ người dùng nào:
* đặc trưng: `tên đăng nhập, mật khẩu, một chuỗi chứa thông tin được mã hóa, thậm chí là một chuỗi ký tự random`
* được gắn vào HTTP qua: : `URL, Header (Cookie header, Authorization header, Custom header), Body (Form field,...)`

## Process
* để có sự thống nhất trước giữa người dùng và ứng dụng giúp nhận dạng được người dùng, authentication sẽ bao gồm 3 phần:
* -> Tạo dấu hiệu: dùng dấu hiệu gì, làm sao tạo ra dấu hiệu đó (username/password, user token, api key,...)
* -> Lưu trữ dấu hiệu: lưu trữ ở đâu, ở cả server và client, thông qua vị trí nào trên HTTP,...
* -> Kiểm tra dấu hiệu: kiểm tra lại tính hợp lệ của dấu hiệu, đối chiếu xem dấu hiệu này là của người dùng nào,...

## Mechanism
* authentication sẽ gồm 2 bước:Xác thực một user (thường là request đầu tiên), giữ đăng nhập (cho các request phía sau)
* một số cơ chế authentication cơ bản: Basic Authentication, Session-based Authentication, Token-based Authentication

## Basic Auth
* gửi chính username + password của người dùng theo mỗi request (cơ chế xác thực đơn giản nhất của một ứng dụng web)

* Được nhiều phần mềm/máy chủ tích hợp và xử lý một cách tự động (nginx, apache,...) -> dễ dàng config cho webserver sử dụng Basic Auth với 1 vài dòng
* Dễ dàng kết hợp với các phương pháp khác (cookie, session, token,...); do đã được xử lý mặc định trên trình duyệt và webserver thông qua truyền tải http header 

* Basic Auth thường được sử dụng trong _`các ứng dụng nội bộ, các thư mục cấm như hệ thống CMS, môi trường development, database admin,...`_; lợi dụng việc chặt chẽ của kiểm tra Basic Auth trên các web server để tránh tiết lộ thông tin hệ thống nội bộ cho người ngoài, phòng chống hack, khai thác lỗ hổng ứng dụng,...

### Process
* Tạo Dấu hiệu: `Chuỗi` username:password đã `được mã hóa` Base64
```
abc:123 --Base64--> YWJjOjEyMw==
```

* Lưu trữ dấu hiệu: 
* -> `Tại server`: Máy chủ web sẽ lưu lại username, password trong database, file (htpasswd),...
* -> `Tại client`: Sau khi hỏi người dùng nhập username và password lần đầu -> browser sẽ lưu lại 2 giá trị này trong bộ nhớ được quản lý bởi mỗi trình duyệt (thời gian lưu thường là có giới hạn và không thể tiếp cận bộ nhớ này bằng code) -> lấy thông tin này đính kèm từng request (tránh phải liên tục hỏi username, password)
* -> `Truyền tải`: chuỗi đã mã hóa base64 phía trên sẽ được truyền trong HTTP request trong `Authorization header` với từ khóa Basic phía trước: "Authorization: Basic YWJjOjEyMw=="

* Kiểm tra dấu hiệu: Với `mỗi request` gửi lên kèm thông tin username/password trên, server sẽ so sánh username/password với database, config file,... để kiểm tra tính hợp lệ

### Nhược điểm
* `Username/password dễ bị lộ` (qua việc bắt request, log server,...)
* `Không thể logout người dùng` ngoại trừ việc tự xóa lịch sử duyệt web hoặc hết thời gian lưu của trình duyệt; Vì việc lưu username, password dưới trình duyệt được thực hiện tự động và không có sự can thiệp của chủ trang web
* `Không thân thiện với người dùng`: Việc hiển thị hộp thoại đăng nhập cũng như thông báo lỗi của trình duyệt rất nhàm chán, không chứa đựng nhiều thông tin cho người dùng

## Session-based Authentication (cookie-based authentication)
* cơ chế đăng nhập người dùng dựa trên việc tạo ra **`session`** của người dùng ở phía server. 
* Sau quá trình xác thực người dùng thành công (username/password,...) thì phía server sẽ tạo và lưu ra một session chứa thông tin của người dùng đang đăng nhập -> trả lại cho client session ID để truy cập session cho những request sau

*  Session-based Authentication thường được dùng trong các `website và những ứng dụng` web làm việc chủ yếu với browser, những `hệ thống monolithic` do cần sự tập trung trong việc lưu session data và sự hạn chế về domain
### Process
* Tạo 1 chuỗi (thường là random) `unique` gọi là **`Session ID`**

* **Tại server**: Lưu dữ liệu của session trong database, file, ram,... và `dùng Session ID để tìm kiếm`
* **Tại client**: Lưu Session ID trong bộ nhớ **`cookie`**, `hoặc URL trang web, form field ẩn,...`
* Truyền tải: Session ID sẽ xuất hiện trong các HTTP request tiếp theo trong Cookie (header Cookie: SESSION_ID=abc), URL (/profile?session_id=abc), body (form field ẩn),...

* Server dùng Session ID client truyền lên để tìm dữ liệu của session từ các nguồn lưu như database, file, ram,...

* Quá trình set Session ID thường được thực hiện một cách tự động bởi server, cho nên session-based authentication thường sử dụng `Cookie`
* vì cookie có thể set được từ phía server và được browser `áp dụng tự động` cho các request tiếp theo. Tuy nhiên vẫn có nhiều cách để sử dụng được session ID mà không dùng cookie 

### Ưu điểm 
* `Thông tin được giấu kín`: Client chỉ được biết tới Session ID thường là 1 chuỗi random không mang thông tin gì của người dùng, còn mọi thông tin khác của phiên đăng nhập hay người dùng hiện tại đều được lưu phía server nên cơ chế này giữ kín được thông tin của người dùng trong quá trình truyền tải.
* `Dung lượng truyền tải nhỏ`: Bởi vì tự thân Session ID không mang theo thông tin gì, thông thường chỉ là một chuỗi ký tự unique khoảng 20-50 ký tự, do vậy việc gắn Session ID vào mỗi request không làm tăng nhiều độ dài request, do đó việc truyền tải sẽ diễn ra dễ dàng hơn.
* `Không cần tác động client`: Theo mình thì để sử dụng cơ chế session này bạn chủ yếu chỉ cần sửa phía server. Client (browser) hầu như không cần phải xử lý gì thêm bởi đã được tích hợp tự động (đối với cookie), hoặc response trả về của server đã có sẵn (đối với session ID ở URL hoặc hidden form)
* `Fully-controlled session`:  cho phép hệ thống quản trị tất cả các hoạt động liên quan tới phiên đăng nhập của người dùng như: thời gian login, force logout,...

### Nhược điểm
* `Chiếm nhiều bộ nhớ`: Với mỗi phiên làm việc của user, server sẽ lại phải tạo ra một session và lưu vào bộ nhớ trên server (số data này có thể còn lớn hơn cả user database của bạn do mỗi user có thể có vài session khác nhau) -> việc tra cứu đối với các hệ thống lớn nhiều người dùng sẽ là vấn đề.
* `Khó scale`: Vì tính chất stateful của việc lưu session data ở phía server, do đó bạn sẽ khó khăn hơn trong việc scale ngang ứng dụng, 
```
// tức nếu ta chạy ứng dụng ở 10 máy chủ (hay 10 container), 
// thì 1 là ta phải dùng chung chỗ lưu session, 
// 2 là nếu không dùng chung bộ nhớ session, thì phải có giải pháp để ghi nhớ user đã kết nối tới server nào 
// Nếu không rất có thể chỉ cần ấn refresh thôi, user kết nối với server khác khi cân bằng tải là sẽ như chưa hề có cuộc login ngay.
```
* `Phụ thuộc domain`: Vì thường sử dụng cookie, mà cookie lại phụ thuộc vào domain, do vậy khả năng sử dụng phiên đăng nhập của bạn sẽ bị giới hạn ở đúng domain được set cookie (không phù hợp với các hệ thống phân tán hoặc tích hợp vào ứng dụng bên thứ 3)
* `CSRF`: Nói nôm na là Session ID thường được lưu vào Cookie, và cookie mới là thứ dễ bị tấn công kiểu này. Vì cookie được tự động gắn vào các request tới domain của bạn. 
```Ví dụ:
// User vừa login vào my-bank.com và được set cookie: session_id=123
// User vào trang web taolahacker.com xem tut của mình
// Trên taolahacker.com mình ngầm gửi 1 request ajax tới domain my-bank.com.
// Vì browser tự động thêm cookie session_id=123 vào request ajax trên, do vậy request của mình có thể thao tác mọi thứ NHƯ USER
```

## Token-based Authentication
* cơ chế đăng nhập người dùng dựa trên việc tạo ra **`token`** (một chuỗi ký tự thường được mã hóa mang thông tin xác định người dùng được server tạo ra và lưu ở client; Server sau đó có thể không lưu lại token này)

* Token-based Authentication thường được sử dụng trong các hệ thống `Web API`, các `hệ thống phân tán`, `micro-services`, các hệ thống có sự tham gia của các nền tảng khác như `mobile, IoT, server,..., hoặc các website kiểu mới` (phân tách rõ UI app và API).

_Signed Cookie là cookie chứa thông tin phiên đăng nhập được mã hóa và signed, là 1 phương pháp cookie-based nhưng xếp vào loại Token-based vì thông tin phiên đăng nhập được lưu hết trên cookie (client) nhé_
### Process
* Tạo 1 chuỗi chứa thông tin người dùng (thường được mã hóa và signed) gọi là token

* `Tại server`: Thường là không cần lưu.
* `Tại client`: Ứng dụng client (javascript, mobile,...) phải tự lưu token trong các bộ nhớ ứng dụng, local storage, cookie,...
* Token sẽ xuất hiện trong các HTTP request tiếp theo trong **`Authorization header`** (Authorization: Bearer abc), Cookie (header Cookie: token=abc), URL (/profile?token=abc), body (ajax body, field),...
* Kiểm tra tính toàn vẹn của token qua signature của token

* Token thường có tính self-contained (như JWT) (tức là có thể tự kiểm tra tính đúng đắn nhờ vào các thuật toán mã hóa và giải mã chỉ dựa vào thông tin trên token và 1 secret key nào đó của server) -> server không cần thiết phải lưu lại token, hay truy vấn thông tin user để xác nhận token

### JWT
* self-contained
* được hỗ trợ bởi nhiều ngôn ngữ và nền tảng 
* hơn hết là cấu trúc JSON đơn giản, nhỏ gọn hơn rất nhiều so với các loại token khác như `Simple Web Tokens (SWT)` hay `Security Assertion Markup Language Tokens (SAML)`

### Ưu điểm
* `Stateless`: Bởi vì token thường có tính chất self-contained, do vậy server không cần lưu thêm thông tin gì về token hay map giữa token và người dùng -> đây là tính chất quan trọng nhất, phục vụ cho việc scale ứng dụng theo chiều ngang khi không cần quan tâm tới việc bạn sẽ sinh ra token ở đâu và verify token ở đâu.
* `Phù hợp với nhiều loại client`: cookie là một concept được các browser áp dụng tự động, còn với các client sử dụng Web API như mobile, IoT device, server,... thì việc sử dụng cookie lại rất hạn chế. Sử dụng token trong header hay URL,... sẽ dễ dàng hơn cho client trong việc lưu lại token và truyền tải token.
* `Chống CSRF`: Do việc sử dụng token phải được client xử lý từ việc lưu tới truyền tải, do vậy sử dụng token (mà không dùng cookie) sẽ phòng chống được các trường hợp tấn công như với trường hợp session/cookie.
* `Không bị giới hạn bởi domain`: Đây là tính chất giúp các hệ thống hiện đại có sự tham gia của bên thứ 3 hoạt động dễ dàng hơn khi không bị giới hạn chỉ ở domain của hệ thống đăng nhập.

### Nhược điểm 
* `Khó quản lý đăng xuất`: Bởi vì server không lưu thông tin gì về token hay session của user, do đó điều khó kiểm soát nhất chính là việc đăng xuất. Và vì việc kiểm tra token chỉ dựa vào thông tin trên token, do vậy sẽ khó để ứng dụng của chúng ta vô hiệu hóa một token vẫn còn hiệu lực.
* `Phức tạp phần client`: Cơ chế sử dụng token thường yêu cầu client phải có xử lý liên quan tới lưu token, gửi token, do vậy sẽ không phù hợp với những website kiểu cũ, sử dụng nhiều server render html và phần javascript hạn chế.
* `Thông tin dễ lộ`: Khác với session, thông tin về phiên đăng nhập của người dùng có trên token và được lưu phía client, do vậy sẽ có các nguy cơ liên quan tới lộ thông tin trong token trong quá trình lưu trữ, truyền tải,... -> thông thường người ta chỉ lưu 1 số thông tin thiết yếu như user_id, username mà không lưu những thông tin nhạy cảm như password vào token.