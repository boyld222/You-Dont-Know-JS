# You Don't Know JS Yet: Bắt Đầu - Ấn bản thứ 2

# Chương 2: Khảo Sát JS

Cách tốt nhất để học JS là bắt đầu viết JS.

Để làm được điều đó, bạn cần biết JS hoạt động như thế nào, và đó là điều chúng ta sẽ tập trung ở đây. Ngay cả khi bạn đã lập trình với ngôn ngữ khác, hãy dành thời gian làm quen với JS, và nhớ thực hành từng phần.

Chương này không phải là tài liệu tham khảo đầy đủ mọi cú pháp của JS. Nó cũng không phải là giáo trình nhập môn JS hoàn chỉnh.

Thay vào đó, chúng ta sẽ khảo sát một số chủ đề lớn của ngôn ngữ. Mục tiêu là cảm nhận rõ hơn về JS, để tự tin hơn khi viết chương trình. Chúng ta sẽ quay lại nhiều chủ đề này với mức độ chi tiết hơn ở các chương sau của cuốn sách và cả series.

Đừng mong chương này là một bài đọc nhanh. Nó dài và có nhiều chi tiết để nghiền ngẫm. Hãy từ từ.

| MẸO:                                                                                                                                                                                                                                                                                                        |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nếu bạn còn đang làm quen với JS, mình khuyên bạn nên dành nhiều thời gian cho chương này. Hãy đọc từng phần, suy ngẫm và thử nghiệm. Xem các chương trình JS có sẵn và so sánh với giải thích (và quan điểm!) ở đây. Bạn sẽ hiểu sâu hơn về bản chất của JS và tận dụng tốt hơn các phần còn lại của sách. |

## Mỗi File Là Một Chương Trình

Hầu hết mọi website (ứng dụng web) bạn dùng đều gồm nhiều file JS khác nhau (thường có đuôi .js). Dễ nghĩ rằng toàn bộ ứng dụng là một chương trình. Nhưng JS lại nhìn khác.

Trong JS, mỗi file độc lập là một chương trình riêng biệt.

Điều này quan trọng chủ yếu ở khía cạnh xử lý lỗi. Vì JS coi mỗi file là một chương trình, nên một file có thể bị lỗi (khi parse/compile hoặc thực thi) mà không nhất thiết ngăn file tiếp theo được xử lý. Tất nhiên, nếu ứng dụng của bạn phụ thuộc vào 5 file .js, và một file bị lỗi, thì ứng dụng tổng thể có thể chỉ hoạt động một phần. Hãy đảm bảo mỗi file đều hoạt động đúng, và nếu có thể, hãy xử lý lỗi ở file khác một cách hợp lý.

Có thể bạn sẽ ngạc nhiên khi coi mỗi file .js là một chương trình JS riêng. Từ góc độ người dùng, nó giống như một chương trình lớn. Đó là vì quá trình thực thi cho phép các _chương trình_ nhỏ này hợp tác và hoạt động như một chương trình duy nhất.

| LƯU Ý:                                                                                                                                                |
| :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nhiều dự án dùng công cụ build để gộp các file riêng lẻ thành một file duy nhất khi đưa lên web. Khi đó, JS coi file gộp này là toàn bộ chương trình. |

Cách duy nhất để nhiều file .js độc lập hoạt động như một chương trình là chia sẻ trạng thái (và truy cập chức năng public) qua "global scope". Chúng trộn lẫn trong namespace global này, nên khi chạy sẽ như một khối thống nhất.

Từ ES6, JS còn hỗ trợ định dạng module bên cạnh kiểu chương trình JS độc lập. Module cũng dựa trên file. Nếu file được load qua cơ chế module như `import` hoặc thẻ `<script type=module>`, toàn bộ mã trong file được coi là một module.

Dù bạn không nghĩ module—một tập hợp trạng thái và các phương thức public thao tác lên trạng thái đó—là một chương trình độc lập, JS thực tế vẫn coi mỗi module là riêng biệt. Tương tự như "global scope" cho phép file độc lập trộn lẫn khi runtime, import module vào file khác cho phép chúng tương tác khi runtime.

Dù bạn tổ chức mã theo kiểu nào (file độc lập hay module), hãy luôn nghĩ mỗi file là một (mini) chương trình, có thể hợp tác với các (mini) chương trình khác để tạo nên ứng dụng tổng thể.

## Giá Trị (Value)

Đơn vị thông tin cơ bản nhất trong chương trình là giá trị (value). Giá trị là dữ liệu. Chúng giúp chương trình lưu trạng thái. Trong JS, giá trị có hai dạng: **primitive** và **object**.

Giá trị được nhúng vào chương trình qua _literal_:

```js
greeting("My name is Kyle.");
```

Ở đây, giá trị `"My name is Kyle."` là một primitive string literal; string là tập hợp ký tự có thứ tự, thường dùng để biểu diễn từ và câu.

Mình dùng dấu nháy kép `"` để _bao_ (delimit) giá trị string. Nhưng mình cũng có thể dùng nháy đơn `'`. Việc chọn loại nháy hoàn toàn là phong cách. Quan trọng là, để mã dễ đọc và bảo trì, hãy chọn một kiểu và dùng nhất quán.

Một lựa chọn khác để bao string literal là dùng dấu back-tick `` ` ``. Tuy nhiên, lựa chọn này không chỉ là phong cách; nó còn khác về hành vi. Xem ví dụ:

```js
console.log("My name is ${ firstName }.");
// My name is ${ firstName }.

console.log("My name is ${ firstName }.");
// My name is ${ firstName }.

console.log(`My name is ${firstName}.`);
// My name is Kyle.
```

Giả sử chương trình đã định nghĩa biến `firstName` với giá trị string `"Kyle"`, thì string dùng back-tick sẽ thay thế biểu thức biến (dùng `${ .. }`) bằng giá trị hiện tại. Đây gọi là **interpolation** (nội suy).

String dùng back-tick có thể không chứa biểu thức nội suy, nhưng như vậy thì mất ý nghĩa của cú pháp này:

```js
console.log(`Am I confusing you by omitting interpolation?`);
// Am I confusing you by omitting interpolation?
```

Cách tốt nhất là dùng `"` hoặc `'` (chọn một và nhất quán!) cho string _trừ khi bạn cần_ nội suy; chỉ dùng `` ` `` khi cần nội suy biến.

Ngoài string, JS còn có các giá trị primitive literal khác như boolean và number:

```js
while (false) {
    console.log(3.141592);
}
```

`while` là một loại vòng lặp, lặp lại thao tác _khi_ điều kiện đúng.

Ở đây, vòng lặp sẽ không chạy (không in gì), vì điều kiện là giá trị boolean `false`. Nếu là `true` thì vòng lặp sẽ chạy mãi, nên hãy cẩn thận!

Số `3.141592` là xấp xỉ số PI toán học đến 6 chữ số. Thay vì tự nhập giá trị này, bạn nên dùng sẵn `Math.PI`. Một biến thể khác là kiểu `bigint` (số nguyên lớn), dùng để lưu số rất lớn.

Số thường dùng để đếm bước lặp, hoặc truy cập vị trí trong mảng (array index). Ví dụ, nếu có mảng `names`, ta truy cập phần tử thứ hai như sau:

```js
console.log(`My name is ${names[1]}.`);
// My name is Kyle.
```

Ta dùng `1` cho phần tử thứ hai, không phải `2`, vì như đa số ngôn ngữ lập trình, chỉ số mảng JS bắt đầu từ 0 (`0` là vị trí đầu).

Ngoài string, number, boolean, còn hai giá trị _primitive_ khác là `null` và `undefined`. Dù có khác biệt (lịch sử và hiện tại), về cơ bản cả hai đều chỉ sự _rỗng_ (hoặc không có) giá trị.

Nhiều lập trình viên thích coi chúng như nhau, tức là đều chỉ rỗng. Nếu cẩn thận, điều này thường làm được. Tuy nhiên, tốt nhất là chỉ dùng `undefined` làm giá trị rỗng duy nhất, dù `null` có vẻ ngắn hơn!

```js
while (value != undefined) {
    console.log("Still got something!");
}
```

Giá trị primitive cuối cùng cần biết là symbol, một giá trị đặc biệt dùng như khóa ẩn không đoán được. Symbol gần như chỉ dùng làm key đặc biệt cho object:

```js
hitchhikersGuide[Symbol("meaning of life")];
// 42
```

Bạn sẽ hiếm khi dùng symbol trực tiếp trong JS thông thường. Chủ yếu chúng dùng ở mã cấp thấp như thư viện, framework.

### Array Và Object

Ngoài primitive, kiểu giá trị còn lại trong JS là object.

Như đã nói, array là một dạng đặc biệt của object, gồm danh sách dữ liệu có thứ tự và chỉ số số học:

```js
var names = ["Frank", "Kyle", "Peter", "Susan"];

names.length;
// 4

names[0];
// Frank

names[1];
// Kyle
```

Array JS có thể chứa bất kỳ kiểu giá trị nào, kể cả primitive hoặc object (kể cả array khác). Như sẽ thấy ở cuối chương 3, thậm chí function cũng là giá trị có thể lưu trong array hoặc object.

| LƯU Ý:                                                                                                   |
| :------------------------------------------------------------------------------------------------------- |
| Function, giống array, là một dạng (sub-type) đặc biệt của object. Sẽ nói kỹ hơn về function ở phần sau. |

Object tổng quát hơn: tập hợp không có thứ tự, mỗi phần tử là một cặp key-value. Tức là bạn truy cập phần tử qua tên key (property) thay vì chỉ số như array. Ví dụ:

```js
var me = {
    first: "Kyle",
    last: "Simpson",
    age: 39,
    specialties: ["JS", "Table Tennis"],
};

console.log(`My name is ${me.first}.`);
```

Ở đây, `me` là object, `first` là tên vị trí thông tin trong object (tập giá trị). Một cú pháp khác để truy cập thông tin trong object là dùng ngoặc vuông `[ ]`, ví dụ `me["first"]`.

### Xác Định Kiểu Giá Trị

Để phân biệt giá trị, toán tử `typeof` cho biết kiểu built-in nếu là primitive, hoặc "object" nếu không:

```js
typeof 42; // "number"
typeof "abc"; // "string"
typeof true; // "boolean"
typeof undefined; // "undefined"
typeof null; // "object" -- lỗi!
typeof { a: 1 }; // "object"
typeof [1, 2, 3]; // "object"
typeof function hello() {}; // "function"
```

| CẢNH BÁO:                                                                                                                                           |
| :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| `typeof null` trả về "object" thay vì "null" như mong đợi. Ngoài ra, `typeof` trả về "function" cho function, nhưng không trả về "array" cho array. |

Chuyển đổi kiểu giá trị, ví dụ từ string sang number, gọi là "coercion" trong JS. Sẽ nói kỹ hơn ở Phụ lục A, "Giá trị vs Tham chiếu".

Primitive và object có hành vi khác nhau khi gán hoặc truyền đi. Sẽ nói chi tiết ở Phụ lục A, "Giá trị vs Tham chiếu".

## Khai Báo Và Sử Dụng Biến

Để làm rõ điều có thể chưa rõ ở phần trước: trong JS, giá trị có thể xuất hiện trực tiếp (literal) hoặc lưu trong biến; hãy coi biến như hộp chứa giá trị.

Biến phải được khai báo (tạo) mới dùng được. Có nhiều cú pháp khai báo biến (identifier), mỗi kiểu có hành vi ngầm định khác nhau.

Ví dụ, xét lệnh `var`:

```js
var myName = "Kyle";
var age;
```

Từ khóa `var` khai báo biến để dùng ở phần mã đó, và có thể gán giá trị khởi tạo.

Một từ khóa tương tự là `let`:

```js
let myName = "Kyle";
let age;
```

Từ khóa `let` có một số điểm khác với `var`, rõ nhất là `let` giới hạn phạm vi truy cập biến hơn `var`. Điều này gọi là "block scoping" (phạm vi khối) thay vì phạm vi hàm (function scoping) như thường thấy.

Xem ví dụ:

```js
var adult = true;

if (adult) {
    var myName = "Kyle";
    let age = 39;
    console.log("Shhh, this is a secret!");
}

console.log(myName);
// Kyle

console.log(age);
// Error!
```

Cố gắng truy cập `age` ngoài khối `if` sẽ gây lỗi, vì `age` chỉ tồn tại trong block `if`, còn `myName` thì không.

Block-scoping rất hữu ích để giới hạn phạm vi biến trong chương trình, giúp tránh trùng tên biến ngoài ý muốn.

Tuy nhiên, `var` vẫn hữu dụng khi bạn muốn biến có phạm vi rộng hơn (toàn bộ hàm). Cả hai kiểu khai báo đều phù hợp tùy hoàn cảnh.

| LƯU Ý:                                                                                                                                                                                                                                                                                                                                                               |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Rất nhiều người khuyên nên tránh dùng `var` mà chỉ dùng `let` (hoặc `const`!), chủ yếu vì sự nhầm lẫn về phạm vi của `var` từ xưa. Mình cho rằng lời khuyên này quá cứng nhắc và không thực sự hữu ích. Nó giả định bạn không thể học và dùng đúng tính năng này kết hợp với các tính năng khác. Mình tin bạn _có thể_ và _nên_ học mọi tính năng, và dùng đúng chỗ! |

Một kiểu khai báo thứ ba là `const`. Nó giống `let` nhưng có thêm ràng buộc: phải gán giá trị ngay khi khai báo, và không thể gán lại giá trị khác sau đó.

Xem ví dụ:

```js
const myBirthday = true;
let age = 39;

if (myBirthday) {
    age = age + 1; // OK!
    myBirthday = false; // Error!
}
```

Hằng số `myBirthday` không được phép gán lại giá trị.

Biến khai báo bằng `const` không phải là "không thay đổi được", mà chỉ là không thể gán lại. Không nên dùng `const` cho object, vì giá trị object vẫn có thể thay đổi dù biến không gán lại được. Điều này dễ gây nhầm lẫn, nên tốt nhất tránh trường hợp như:

```js
const actors = ["Morgan Freeman", "Jennifer Aniston"];

actors[2] = "Tom Cruise"; // OK :(
actors = []; // Error!
```

Cách dùng `const` hợp lý nhất là với giá trị primitive đơn giản mà bạn muốn đặt tên ý nghĩa, ví dụ dùng `myBirthday` thay cho `true`. Điều này giúp mã dễ đọc hơn.

| MẸO:                                                                                                                                                                                  |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Nếu bạn chỉ dùng `const` cho giá trị primitive, bạn sẽ tránh được nhầm lẫn giữa gán lại (không cho phép) và thay đổi giá trị (cho phép)! Đó là cách dùng `const` an toàn và tốt nhất. |

Ngoài `var` / `let` / `const`, còn nhiều cú pháp khác khai báo biến (identifier) ở các phạm vi khác nhau. Ví dụ:

```js
function hello(myName) {
    console.log(`Hello, ${myName}.`);
}

hello("Kyle");
// Hello, Kyle.
```

Identifier `hello` được tạo ở phạm vi ngoài, và tự động tham chiếu đến function. Tham số `myName` chỉ tồn tại trong function, chỉ truy cập được trong phạm vi đó. `hello` và `myName` thường hoạt động như biến khai báo bằng `var`.

Một cú pháp khác khai báo biến là trong `catch`:

```js
try {
    someError();
} catch (err) {
    console.log(err);
}
```

Biến `err` chỉ tồn tại trong block `catch`, như thể được khai báo bằng `let`.

## Hàm (Function)

Từ "function" có nhiều nghĩa trong lập trình. Ví dụ, trong Functional Programming, "function" có định nghĩa toán học chặt chẽ và quy tắc nghiêm ngặt.

Trong JS, nên hiểu "function" theo nghĩa rộng hơn, gần với "procedure" (thủ tục). Thủ tục là tập hợp các câu lệnh có thể gọi nhiều lần, nhận đầu vào và trả về đầu ra.

Từ những ngày đầu của JS, khai báo function như sau:

```js
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}
```

Đây gọi là function declaration vì nó là một statement độc lập, không phải expression trong statement khác. Việc gán identifier `awesomeFunction` với function diễn ra ở giai đoạn compile, trước khi chạy mã.

Khác với function declaration, function expression được định nghĩa và gán như sau:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function (coolThings) {
    // ..
    return amazingStuff;
};
```

Đây là function expression gán cho biến `awesomeFunction`. Khác với function declaration, function expression chỉ gán cho identifier khi thực thi đến dòng đó.

Điều cực kỳ quan trọng là trong JS, function là giá trị có thể gán (như ví dụ trên) và truyền đi. Thực tế, function JS là một dạng đặc biệt của object. Không phải ngôn ngữ nào cũng coi function là giá trị, nhưng đây là điều kiện cần để hỗ trợ functional programming, như JS làm.

Function JS có thể nhận tham số đầu vào:

```js
function greeting(myName) {
    console.log(`Hello, ${myName}!`);
}

greeting("Kyle"); // Hello, Kyle!
```

Ở đây, `myName` là tham số, hoạt động như biến cục bộ trong function. Function có thể nhận bất kỳ số lượng tham số nào, từ 0 trở lên. Mỗi tham số nhận giá trị đối số truyền vào ở vị trí tương ứng (`"Kyle"` ở đây).

Function cũng có thể trả về giá trị với từ khóa `return`:

```js
function greeting(myName) {
    return `Hello, ${myName}!`;
}

var msg = greeting("Kyle");

console.log(msg); // Hello, Kyle!
```

Bạn chỉ có thể `return` một giá trị, nhưng nếu muốn trả nhiều giá trị, có thể gói vào object/array.

Vì function là giá trị, chúng có thể gán làm property của object:

```js
var whatToSay = {
    greeting() {
        console.log("Hello!");
    },
    question() {
        console.log("What's your name?");
    },
    answer() {
        console.log("My name is Kyle.");
    },
};

whatToSay.greeting();
// Hello!
```

Ở đây, ba function (`greeting()`, `question()`, `answer()`) là property của object `whatToSay`. Mỗi function có thể gọi qua property tương ứng. So sánh cách định nghĩa function trên object này với cú pháp `class` sẽ nói sau trong chương này.

Function trong JS có nhiều dạng khác nhau. Sẽ nói kỹ hơn ở Phụ lục A, "Quá Nhiều Dạng Function".

## So Sánh (Comparisons)

Để ra quyết định trong chương trình, cần so sánh giá trị để xác định chúng giống hay liên quan thế nào. JS có nhiều cơ chế so sánh giá trị, hãy cùng xem kỹ hơn.

### Gần Bằng (Equal...ish)

So sánh phổ biến nhất trong JS là hỏi: "Giá trị X này _có giống_ giá trị Y kia không?" Nhưng "giống" nghĩa là gì với JS?

Vì lý do lịch sử và tiện dụng, ý nghĩa này phức tạp hơn so với so khớp _đúng hệt_ như trực giác. Đôi khi so sánh equality là _đúng hệt_, nhưng đôi khi lại rộng hơn, cho phép _gần giống_ hoặc _có thể thay thế_. Nói cách khác, cần phân biệt giữa so sánh **equality** (bằng) và **equivalence** (tương đương).

Nếu bạn từng làm việc với JS, chắc chắn đã thấy toán tử "ba dấu bằng" `===`, còn gọi là "strict equality". Nghe có vẻ đơn giản: "strict" là nghiêm ngặt, tức là _đúng hệt_.

Nhưng không hẳn vậy.

Đúng là đa số giá trị khi so sánh bằng `===` sẽ đúng như trực giác _đúng hệt_ đó. Xem ví dụ:

```js
3 === 3.0; // true
"yes" === "yes"; // true
null === null; // true
false === false; // true

42 === "42"; // false
"hello" === "Hello"; // false
```

| LƯU Ý:                                                                                                                                                                                                                                                                                                                                                                                                  |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Một cách khác để mô tả so sánh bằng của `===` là "so sánh cả giá trị và kiểu dữ liệu". Trong các ví dụ như `42 === "42"`, _kiểu_ của hai giá trị (number, string, v.v.) dường như là yếu tố quyết định. Tuy nhiên, **mọi** phép so sánh giá trị trong JS đều xét đến kiểu, không chỉ riêng `===`. Cụ thể, `===` không cho phép chuyển đổi kiểu (coercion), còn các phép so sánh khác thì _có_ cho phép. |

Nhưng toán tử `===` cũng có một số điểm tinh vi mà nhiều lập trình viên JS thường bỏ qua, gây ra hậu quả không mong muốn. `===` được thiết kế để _nói dối_ trong hai trường hợp đặc biệt: `NaN` và `-0`. Xem ví dụ:

```js
NaN === NaN; // false
0 === -0; // true
```

Với `NaN`, toán tử `===` _nói dối_ rằng một giá trị `NaN` không bằng một giá trị `NaN` khác. Với `-0` (đây là một giá trị thực sự tồn tại và có thể dùng trong JS!), `===` _nói dối_ rằng nó bằng với giá trị `0` thông thường.

Vì sự _nói dối_ này có thể gây rắc rối, tốt nhất là tránh dùng `===` cho các trường hợp này. Để so sánh `NaN`, hãy dùng `Number.isNaN(..)`, hàm này không nói dối. Để so sánh `-0`, hãy dùng `Object.is(..)`, hàm này cũng không nói dối. `Object.is(..)` cũng có thể dùng để kiểm tra `NaN` một cách "trung thực" nếu bạn muốn. Vui nhộn là, bạn có thể coi `Object.is(..)` như "bốn dấu bằng" `====`, tức là so sánh cực kỳ nghiêm ngặt!

Có những lý do kỹ thuật và lịch sử sâu xa cho các trường hợp _nói dối_ này, nhưng điều đó không thay đổi thực tế rằng `===` không thực sự là so sánh _nghiêm ngặt tuyệt đối_.

Câu chuyện còn phức tạp hơn khi so sánh giá trị object (không phải primitive). Xem ví dụ:

```js
[1, 2, 3] === [1, 2, 3]; // false
{ a: 42 } === { a: 42 }; // false
(x => x * 2) === (x => x * 2); // false
```

Chuyện gì đang xảy ra vậy?

Có thể bạn nghĩ rằng phép so sánh sẽ xét đến _bản chất_ hoặc _nội dung_ của giá trị; ví dụ, `42 === 42` xét giá trị thực sự và so sánh. Nhưng với object, so sánh dựa trên nội dung gọi là "structural equality" (bằng cấu trúc).

JS không định nghĩa `===` là so sánh bằng cấu trúc cho object. Thay vào đó, `===` dùng so sánh _bằng định danh_ (identity equality) cho object.

Trong JS, mọi giá trị object đều được lưu bằng tham chiếu (xem "Giá trị vs Tham chiếu" ở Phụ lục A), được gán và truyền đi bằng bản sao tham chiếu, **và** khi so sánh cũng là so sánh bằng tham chiếu (identity). Xem ví dụ:

```js
var x = [1, 2, 3];

// gán bằng bản sao tham chiếu, nên
y tham chiếu cùng một mảng với x,
// không phải bản sao khác.
var y = x;

y === x; // true
y === [1, 2, 3]; // false
x === [1, 2, 3]; // false
```

Ở đây, `y === x` là true vì cả hai biến cùng tham chiếu đến một mảng ban đầu. Nhưng các phép so sánh `=== [1,2,3]` đều false vì `y` và `x` được so với mảng mới hoàn toàn khác `[1,2,3]`. Cấu trúc và nội dung mảng không quan trọng, chỉ có **định danh tham chiếu** mới quan trọng.

JS không cung cấp cơ chế so sánh bằng cấu trúc cho object, chỉ có so sánh bằng định danh tham chiếu. Nếu muốn so sánh bằng cấu trúc, bạn phải tự kiểm tra.

Nhưng hãy cẩn thận, việc này phức tạp hơn bạn nghĩ. Ví dụ, làm sao xác định hai function là "tương đương về cấu trúc"? Ngay cả khi so sánh chuỗi mã nguồn cũng không xét đến closure. JS không cung cấp so sánh bằng cấu trúc vì rất khó xử lý hết các trường hợp đặc biệt!

### So Sánh Ép Kiểu (Coercive Comparisons)

Coercion nghĩa là giá trị một kiểu được chuyển sang biểu diễn ở kiểu khác (nếu có thể). Như sẽ nói ở Chương 4, coercion là một trụ cột của JS, không phải tính năng tùy chọn có thể tránh.

Nhưng khi coercion gặp các toán tử so sánh (như equality), thường gây nhầm lẫn và khó chịu.

Ít có tính năng nào của JS bị chỉ trích nhiều như toán tử `==`, thường gọi là "so sánh lỏng lẻo" (loose equality). Đa số các bài viết và thảo luận công khai về JS đều lên án toán tử này là thiết kế tệ và nguy hiểm khi dùng trong chương trình JS. Ngay cả cha đẻ ngôn ngữ, Brendan Eich, cũng từng thừa nhận đây là một sai lầm lớn.

Theo mình thấy, phần lớn sự khó chịu này đến từ một số ít trường hợp góc khó hiểu, nhưng vấn đề sâu xa hơn là hiểu lầm phổ biến rằng toán tử này so sánh mà không xét đến kiểu dữ liệu.

Toán tử `==` so sánh equality gần giống như `===`. Thực tế, cả hai đều xét đến kiểu dữ liệu của giá trị so sánh. Nếu hai giá trị cùng kiểu, `==` và `===` **hoàn toàn giống nhau, không khác gì cả.**

Nếu kiểu dữ liệu khác nhau, `==` khác `===` ở chỗ nó cho phép ép kiểu trước khi so sánh. Nói cách khác, cả hai đều muốn so sánh giá trị cùng kiểu, nhưng `==` cho phép chuyển đổi kiểu _trước_, và sau khi kiểu đã giống nhau, `==` hoạt động như `===`. Thay vì gọi là "so sánh lỏng lẻo", nên gọi `==` là "so sánh ép kiểu" (coercive equality).

Xem ví dụ:

```js
42 == "42"; // true
1 == true; // true
```

Ở cả hai phép so sánh, kiểu dữ liệu khác nhau, nên `==` sẽ ép các giá trị không phải số (`"42"` và `true`) thành số (`42` và `1`) trước khi so sánh.

Chỉ cần hiểu bản chất này của `==`—rằng nó ưu tiên so sánh số—bạn sẽ tránh được đa số trường hợp rắc rối, ví dụ như tránh so sánh `"" == 0` hoặc `0 == false`.

Bạn có thể nghĩ: "Vậy mình sẽ luôn tránh so sánh ép kiểu (dùng `===` thay cho `==`) để tránh các trường hợp góc!" Nhưng thực tế không đơn giản như vậy.

Khả năng cao là bạn sẽ dùng các toán tử so sánh thứ tự như `<`, `>` (và cả `<=`, `>=`).

Cũng như `==`, các toán tử này sẽ hoạt động "nghiêm ngặt" nếu kiểu dữ liệu đã giống nhau, nhưng sẽ ép kiểu trước (thường là sang số) nếu kiểu khác nhau.

Xem ví dụ:

```js
var arr = ["1", "10", "100", "1000"];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // sẽ chạy 3 lần
}
```

So sánh `i < arr.length` là "an toàn" vì cả hai đều là số. Nhưng `arr[i] < 500` sẽ ép kiểu, vì `arr[i]` là string. Các phép so sánh sẽ thành `1 < 500`, `10 < 500`, `100 < 500`, `1000 < 500`. Đến lần thứ tư là false, nên vòng lặp dừng sau 3 lần.

Các toán tử này thường dùng so sánh số, trừ khi **cả hai** giá trị đều là string; khi đó sẽ so sánh theo thứ tự từ điển:

```js
var x = "10";
var y = "9";

x < y; // true, chú ý!
```

Không có cách nào để các toán tử này tránh ép kiểu, trừ khi bạn luôn dùng giá trị cùng kiểu. Đó là mục tiêu tốt, nhưng thực tế vẫn có lúc kiểu khác nhau.

Cách khôn ngoan là không tránh so sánh ép kiểu, mà hãy học và hiểu rõ bản chất của chúng.

So sánh ép kiểu còn xuất hiện ở chỗ khác trong JS, như trong điều kiện (`if`, v.v.), sẽ nói kỹ hơn ở Phụ lục A, "So Sánh Điều Kiện Ép Kiểu".

## Tổ Chức Mã Trong JS

Hai mô hình lớn để tổ chức mã (dữ liệu và hành vi) được dùng rộng rãi trong JS: class và module. Hai mô hình này không loại trừ nhau; nhiều chương trình dùng cả hai. Một số chương trình chỉ dùng một, thậm chí không dùng cái nào!

Ở một số khía cạnh, hai mô hình này rất khác nhau. Nhưng thú vị là, ở khía cạnh khác, chúng chỉ là hai mặt của một đồng xu. Thành thạo JS đòi hỏi bạn hiểu cả hai mô hình và biết khi nào nên dùng (và khi nào không!).

### Class

Các thuật ngữ "object-oriented", "class-oriented" và "class" đều rất nhiều chi tiết và sắc thái; không có định nghĩa chung cho tất cả.

Ở đây, mình sẽ dùng định nghĩa phổ biến, quen thuộc với ai từng học ngôn ngữ "object-oriented" như C++ hay Java.

Class trong chương trình là định nghĩa một "kiểu" cấu trúc dữ liệu tùy chỉnh, gồm cả dữ liệu và hành vi thao tác lên dữ liệu đó. Class định nghĩa cách cấu trúc dữ liệu hoạt động, nhưng bản thân class không phải là giá trị cụ thể. Để có giá trị cụ thể dùng trong chương trình, class phải được _khởi tạo_ (bằng từ khóa `new`) một hoặc nhiều lần.

Xem ví dụ:

```js
class Page {
    constructor(text) {
        this.text = text;
    }

    print() {
        console.log(this.text);
    }
}

class Notebook {
    constructor() {
        this.pages = [];
    }

    addPage(text) {
        var page = new Page(text);
        this.pages.push(page);
    }

    print() {
        for (let page of this.pages) {
            page.print();
        }
    }
}

var mathNotes = new Notebook();
mathNotes.addPage("Arithmetic: + - * / ...");
mathNotes.addPage("Trigonometry: sin cos tan ...");

mathNotes.print();
// ..
```

Trong class `Page`, dữ liệu là chuỗi text lưu ở thuộc tính `this.text`. Hành vi là phương thức `print()`, in text ra console.

Với class `Notebook`, dữ liệu là mảng các instance của `Page`. Hành vi là `addPage(..)`, tạo mới `Page` và thêm vào danh sách, cùng với `print()` (in ra tất cả các trang).

Câu lệnh `mathNotes = new Notebook()` tạo một instance của class `Notebook`, và `page = new Page(text)` là nơi tạo instance của class `Page`.

Hành vi (method) chỉ gọi được trên instance (không phải class), ví dụ `mathNotes.addPage(..)` và `page.print()`.

Cơ chế `class` cho phép đóng gói dữ liệu (`text`, `pages`) cùng với hành vi (`addPage(..)`, `print()`). Chương trình tương tự có thể viết mà không cần `class`, nhưng sẽ khó tổ chức, khó đọc, dễ lỗi và khó bảo trì hơn.

#### Kế Thừa (Class Inheritance)

Một khía cạnh nữa của thiết kế "class-oriented" truyền thống, dù ít dùng hơn trong JS, là "kế thừa" (inheritance) và "đa hình" (polymorphism). Xem ví dụ:

```js
class Publication {
    constructor(title, author, pubDate) {
        this.title = title;
        this.author = author;
        this.pubDate = pubDate;
    }

    print() {
        console.log(`
            Title: ${this.title}
            By: ${this.author}
            ${this.pubDate}
        `);
    }
}
```

Class `Publication` định nghĩa một tập hành vi chung mà mọi ấn phẩm có thể cần.
