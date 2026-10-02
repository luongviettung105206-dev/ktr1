Câu 1: Trình bày sự khác nhau giữa Value Types (Kiểu giá trị) và Reference Types (Kiểu tham chiếu) trong C# về cơ chế lưu trữ vùng nhớ (Stack vs Heap).
    Trong C#, Value Type (kiểu giá trị) là kiểu dữ liệu lưu trực tiếp giá trị của biến. Các biến Value Type thường được lưu trên Stack. Khi gán một biến cho biến khác, 
    giá trị sẽ được sao chép nên hai biến hoạt động độc lập. Ví dụ: int, float, double, bool, struct, enum.
    Reference Type (kiểu tham chiếu) là kiểu dữ liệu lưu tham chiếu đến một đối tượng. Đối tượng thường được lưu trên Heap, còn biến chứa tham chiếu có thể nằm trên
    Stack. Khi gán một biến Reference Type cho biến khác, tham chiếu được sao chép nên hai biến có thể cùng trỏ đến một đối tượng. Vì vậy, thay đổi đối tượng thông 
    qua một biến thì biến còn lại cũng có thể thấy sự thay đổi. Ví dụ: class, string, array, object.
    Value Type lưu trực tiếp giá trị, còn Reference Type lưu tham chiếu đến đối tượng. Khi gán, Value Type sao chép giá trị, còn Reference Type sao chép tham chiếu
Câu 2:
1. Sự khác biệt giữa init và set thông thường
Thời điểm gán giá trị:
Thuộc tính set thông thường: Cho phép gán hoặc thay đổi giá trị của thuộc tính ở bất kỳ thời điểm nào trong suốt vòng đời của đối tượng.
Thuộc tính init: Chỉ cho phép gán giá trị tại thời điểm khởi tạo đối tượng (qua Constructor hoặc Object Initializer { ... }). Sau khi quá trình khởi tạo hoàn tất, thuộc tính sẽ bị khóa và trở thành read-only.
Tính bất biến (Immutability):
Thuộc tính set thông thường: Không đảm bảo tính bất biến. Dữ liệu của đối tượng có thể bị chỉnh sửa vô tình hoặc cố ý từ bên ngoài.
Thuộc tính init: Đảm bảo tính bất biến cho đối tượng. Giúp tránh các lỗi phát sinh do thay đổi trạng thái (state mutation) ngoài ý muốn.
Khả năng sử dụng với Object Initializer:
Thuộc tính set thông thường: Dùng tốt với Object Initializer.
Thuộc tính init: Cho phép dùng Object Initializer mà không cần tạo Constructor chứa đầy đủ tham số. Đây là ưu điểm vượt trội so với việc dùng thuộc tính chỉ có get (chỉ cho gán qua Constructor).

2. Các trường hợp sử dụng thực tế (Use Cases)
Thiết kế DTO (Data Transfer Object) và Command:
Các đối tượng DTO hoặc Command trong kiến trúc CQRS chỉ làm nhiệm vụ vận chuyển dữ liệu giữa các tầng hệ thống. Dữ liệu này sau khi nhận về không nên bị sửa đổi ở giữa chừng.
Làm việc trong môi trường Đa luồng (Multi-threading / Concurrency):
Đối tượng bất biến (Immutable Object) hoàn toàn Thread-safe (an toàn khi dùng trên nhiều luồng cùng lúc) mà không cần dùng đến cơ chế khóa (lock), giúp tăng hiệu năng và tránh lỗi Race Condition.
Làm Key cho Dictionary hoặc HashSet:
Nếu một đối tượng được dùng làm Key trong Dictionary hoặc phần tử trong HashSet, việc thay đổi các thuộc tính tạo nên mã Hash (GetHashCode) sau khi thêm vào tập hợp sẽ làm hỏng cấu trúc tìm kiếm. Dùng init đảm bảo các thuộc tính này không bị sửa đổi.
Kết hợp với record trong C#:
Kiểu dữ liệu record trong C# 9+ mặc định sử dụng các positional property dưới dạng init để tạo nên các mô hình dữ liệu chuẩn tính chất Value-based Equality và Immutability.
Câu 3:
 Phân biệt virtual và override
Vị trí khai báo:
virtual: Được sử dụng ở lớp cha (Base class).
override: Được sử dụng ở lớp con (Derived class).
Mục đích sử dụng:
virtual: Cho phép và cấp quyền cho các lớp con có thể ghi đè (thay đổi) lại nội dung logic của phương thức này nếu cần.
override: Khai báo rõ ràng rằng lớp con đang chủ động viết lại (ghi đè) logic của phương thức đã được đánh dấu là virtual từ lớp cha.
Yêu cầu về phần thân hàm (Method Body):
virtual: Bắt buộc phải có phần thân hàm (chứa logic triển khai mặc định của lớp cha). Đây là điểm khác biệt lớn so với phương thức trong interface hoặc abstract class.
override: Bắt buộc phải có phần thân hàm chứa logic triển khai mới riêng cho lớp con.
Tính bắt buộc triển khai ở lớp con:
virtual: Không bắt buộc. Nếu lớp con không dùng override, nó sẽ tự động tái sử dụng (kế thừa) logic mặc định từ lớp cha.
override: Chỉ xuất hiện khi lớp con thực sự muốn thay đổi hành vi của phương thức từ lớp cha.
Cơ chế liên kết (Binding Mechanism):
Cả hai đều hoạt động dựa trên cơ chế Dynamic Binding (Late Binding / Liên kết động) thông qua bảng phương thức ảo (V-Table). Khi chương trình chạy, hệ thống sẽ kiểm tra kiểu đối tượng thực sự được tạo ra trên bộ nhớ Heap để gọi đúng phương thức đã override thay vì dựa vào kiểu của biến tham chiếu.
Câu 4:
1. Sự khác biệt về Quyền sở hữu và Vùng nhớ
Thành phần static thuộc về Lớp (Class level):
Thành phần static chỉ tồn tại duy nhất một bản sao (single instance) trong suốt thời gian ứng dụng chạy.
Nó được nạp vào bộ nhớ (vùng nhớ Type Segment / High Frequency Heap) ngay khi Lớp đó được tải bởi CLR (.NET Common Language Runtime), trước khi bất kỳ đối tượng nào được tạo bằng new.
Thành phần Instance thuộc về Đối tượng (Instance level):
Các thành phần không có static (Instance member) được cấp phát bộ nhớ riêng trên Heap mỗi khi bạn dùng toán tử new. Nếu tạo 1,000 đối tượng, sẽ có 1,000 bản sao dữ liệu instance riêng biệt.
Vì thành phần static không thuộc quyền sở hữu của bất kỳ đối tượng cụ thể nào, việc truy xuất nó qua một đối tượng tạo bởi new là không đúng về mặt ngữ nghĩa và quản lý vùng nhớ.

2. Triết lý thiết kế của C# và Tránh mơ hồ (Ambiguity)
C# là ngôn ngữ quản lý kiểu dữ liệu chặt chẽ (Strongly-typed language). Việc C# bắt buộc gọi thành phần static qua tên Lớp (ví dụ: Math.Sqrt(), ClassName.StaticMethod()) mang lại 3 lợi ích quan trọng:
Tránh hiểu lầm về mặt logic: Nếu C# cho phép objectInstance.StaticMethod(), lập trình viên dễ lầm tưởng rằng phương thức đó đang làm việc hoặc làm thay đổi trạng thái nội bộ của riêng objectInstance đó.
Tối ưu hóa thời gian biên dịch (Compile-time resolution): Trình biên dịch C# biết chính xác phương thức static nằm ở đâu thông qua tên Lớp mà không cần thông qua con trỏ đối tượng (không cần kiểm tra null ở thời gian chạy).
Tránh lỗi NullReferenceException không đáng có: Nếu cho phép gọi static qua instance, giả sử biến đối tượng obj = null;, câu lệnh obj.StaticMethod() sẽ gây bối rối: Liệu nó nên ném ra lỗi NullReferenceException hay vẫn chạy bình thường vì thành phần static không phụ thuộc vào dữ liệu trong obj? Bằng cách cấm hoàn toàn cú pháp này, C# loại bỏ tận gốc sự mơ hồ trên.
