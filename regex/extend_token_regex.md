# Extend Token Regex

## 1. Anchors ( Mỏ neo)

|Cú pháp|Ví dụ|Giải thích|
|---|:---:|---|
|`^`|`/^\d+/`</br></br>Candy -> fail</br> 911 -> pass|Trùng khớp với kí tự được của đầu chuỗi|
|`$`|`/\d$/`</br></br>Candy ->fail</br>Candy1 -> pass|Trùng khớp với kí tự của cuối chuỗi|
|`\A`|`/\A\d+/`</br></br>Candy -> fail</br>911 -> pass|Giống với `^`, nhưng có ảnh hưởng đến nhiều dòng|
|`\z`|`/\d\z/`</br></br>Candy ->fail</br>Candy1 -> pass|Giống với `$`, nhưng có ảnh hưởng đến nhiều dòng|
|`\Z`|`/\d\Z/`</br></br>Candy ->fail</br>Candy1 -> pass|Giống với `\z`, nhưng nhận thêm cả khoảng trắng ở dòng tiếp theo|
|`\b`|`/dy\b/`</br></br>Candy -> pass</br>Candy1 -> fail</br></br>`/\bCandy\b/`</br></br>Candy -> pass</br>1Candy -> fail|Trùng khớp với kí tự đứng đằng trước nó</br></br></br></br></br>Ngoài ra, nếu xuất hiện 2 thẻ này, nó sẽ tạo ra bao đóng, giống với khi dùng `^...$`|
|`\B`|`/dy\B/`</br></br>Candy1 -> pass</br>Candy -> fail|Ngược với `\b`, trùng khớp với kí tự mà đằng trước nó không thỏa mãn|

## 2.Group Constructs (Gom nhóm)

|Cú pháp|Ví dụ|Giải thích|
|---|:---:|---|
|`(...)`|`/(foobar)/`|Trùng khớp với giá trị bên trong `(...)` và gom nhóm|
|`(a\|b)`|`/(foo\|bar)/`|Trùng khớp với giá trị bên trong `(a\|b)` và gom vào 1 nhóm|
|`(?:...)`|`(?:foobar)`|Giống với `(...)` nhưng không gom nhóm|
|`(?’name’...)`|`/(?'foobar'first)/`|Giống với `(...)` nhưng đặt tên cho nhóm đó thay vì sử dụng số thứ tự|
|`(?<name>...)`|`/(?<foobar>first)/`|Giống `(?’name’...)`|
|`(?=...)`|`/foo(?=bar)/`</br></br>foobar -> pass</br>foobaz -> pass|Trùng khớp với `foo` nếu đằng sau `foo` là `bar` và `bar` sẽ không phải là một phần của kết quả|
|`(?!...)`|`/foo(?!bar)/`</br></br>foobaz -> pass</br>foobar -> pass|Trùng khớp với `foo` nếu đằng sau `foo` không phải là `bar` và `bar` sẽ không phải là một phần của kết quả|
|`(?<=...)`|`/(?<=foo)bar/`</br></br>foobar -> pass</br>foObar -> fail|Trùng khớp với `bar` nếu đằng trước `bar` là `foo` và `foo` sẽ không phải là một phần của kết quả|
|`(?<!...)`|`/(?<!foo)bar/`</br></br>foObar -> pass</br>foobar -> fail|Trùng khớp với `bar` nếu đằng trước `bar` không phải là `foo` và `foo` sẽ không phải là một phần của kết quả|
|`(?(?=...)yes\|no)`|`(?(?=is)(is delicious)\|(disgusting))`</br></br>Candy is delicious -> pass</br>Candy is disgusting -> fail|Trùng khớp đằng trước là `is` thì phải lấy đúng chuỗi giá trị `is delicious`, nếu sai sẽ lấy chuỗi `disgusting`|
|`(?(?<=...)yes\|no)`|`(?(?<=is)(is delicious)\|(disgusting))`</br></br>Candy is disgusting -> pass</br>Candy is delicious -> fail|Trùng khớp nếu là `is` thì phải lấy đúng chuỗi giá trị `is delicious`, nếu sai sẽ lấy chuỗi `disgusting`|

## 3. Meta Sequences

|Cú pháp|Ví dụ|Giải thích|
|---|:---:|---|
|`\X`|`/X+/`</br></br>äny únicød3 character -> pass|Trùng khớp với các kí tự Unicode, bao gồm cả khoảng trắng|
|`\n`|`/(.)\n/`</br></br>Swimming -> pass</br>Candy -> fail|Sử dụng lại group số `n`, với n là số tự nhiên lớn hơn 0|
|`\g’name’`|`/(?'letter'a).*(\g<letter>)/`</br></br>a plus a -> pass</br>a pluss c -> fail|Sử dụng lại group có tên là `name`|
|`\g<name>`||Giống với `\g’name’`|
|`\xYY`|`/\xD4/`</br></br>Ô -> pass</br>ô -> fail|Trùng khớp với kí tự 8 bit kiểu giá trị hex, [tham khảo](https://unicode-table.com/en/)|
|`\x{YYYY}`|`/\x{2AC}/`</br></br>ʬ -> pass</br>Ô -> fail|Trùng khớp với kí tự 16 bit kiểu giá trị hex, [tham khảo](https://unicode-table.com/en/)|

## 4. Flags

|Cú pháp|Ví dụ|Giải thích|
|---|:---:|---|
|`g`|`/\w+/g`|Cờ global, sẽ tìm hết tất cả các giá trị có thể tìm thấy|
|`m`|`\w+/m`|Cờ multiline, tìm giá trị trong nhiều dòng|
|`s`|`\w+/s`|Cờ single line, ngược với cờ multiline |
|`i`|`/[iou]/i`</br></br>iou -> pass</br>IOU -> pass|Cờ insensitive, nhận cả giá trị chữ thường và chữ in hoa|
