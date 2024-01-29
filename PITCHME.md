---
marp: true
title: Workshop ASP
description: Làm thế nào để viết code rõ ràng, tối ưu và dễ bảo trì 
theme: uncover
transition: fade
paginate: true
_paginate: false
---

![bg opacity](./assets/gradient.jpg)

# Cách viết code rõ ràng, tối ưu và dễ bảo trì

<!-- This is presenter note. You can write down notes through HTML comment. -->

---

![bg opacity](./assets/gradient.jpg)

### Mục tiêu của workshop:

Giới thiệu về nguyên tắc **[SOILD](https://en.wikipedia.org/wiki/SOLID)** trong việc coding giúp cho việc bảo trì và nâng cấp dễ dàng hơn.

<style scoped>p { text-align: left; }</style>

---

![bg opacity](./assets/gradient.jpg)

### Nguyên tắc **[SOILD](https://en.wikipedia.org/wiki/SOLID)**

**[SOILD](https://en.wikipedia.org/wiki/SOLID)** là từ viết tắt dễ nhớ được đặt bởi **[Uncle Bob](https://en.wikipedia.org/wiki/Robert_C._Martin)** vào đầu những năm 2000.

<style scoped>p { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)
Nó đại điện cho một nhóm 5 nguyên tắc:

- **Nguyên tắc đơn nhiệm** (Single Responsibility Principle).
- **Nguyên tắc đóng mở** (Open/Closed Principle).
- **Nguyên tắc thay thế Liskov** (Liskov Substitution Principle).
- **Nguyên tắc phân tách giao diện** (Interface Segregation Principle).
- **Nguyên tắc nghịch đảo phụ thuộc** (Dependency Inversion Principle).

---

![bg opacity](./assets/gradient.jpg)

### 1. Nguyên tắc đơn nhiệm (Single Responsibility Principle - SRP)

- Mỗi một class (lớp) chỉ nên làm một chức năng nhất định. Phải xác định class đó có vai trò, chức năng là gì.

- Nguyên tắc SRP có ý nghĩa quan trọng trong phát triển phần mềm vì nó giúp giảm thiểu rủi ro thay đổi, tăng tính linh hoạt và bảo trì.

<style scoped> { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)

```` js
// Đúng nguyên tắc
function calcPerimeterRectangle(width: number, height: number) {
  return 2 * (width + height);
}

// Sai nguyên tắc
function calcPerimeterAndAreaOfRectangle(width: number, height: number) {
  const perimeter = 2 * (width + height);
  const rectangle = width * height
  return [perimeter, rectangle];
}
````
<!-- 
Trong ví dụ này, hàm `calcPerimeterAndAreaOfRectangle` không chỉ tính toán chu vi của hình chữ nhật mà còn tính toán diện tích. 

Điều này vi phạm SRP vì hàm đang thực hiện hai nhiệm vụ khác nhau: tính diện tích và tính chu vi. Điều này làm cho hàm điều này làm cho hàm trở nên không rõ ràng và khó bảo trì.

Nếu có một thay đổi trong cách tính diện tích hoặc chu vi của hình chữ nhật, ta sẽ phải sửa đổi hàm này, điều này có thể dẫn đến các rủi ro không mong muốn khác.
-->
---

![bg opacity](./assets/gradient.jpg)

### 2. Nguyên tắc đóng mở (Open/Closed Principle - OCP)

- Theo nguyên lý này, mỗi khi ta muốn thêm chức năng,.. cho chương trình, chúng ta nên viết class mới mở rộng từ class cũ (bằng cách kế thừa hoặc sở hữu class cũ) không nên sửa đổi class cũ.

<style scoped> { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)

```js
// Định nghĩa một interface cho hình chữ nhật
interface Rectangle {
  width: number;
  height: number;
}

// Hàm tính chu vi của hình chữ nhật
function calcPerimeterRectangle(rectangle: Rectangle): number {
  return 2 * (rectangle.width + rectangle.height);
}
```

---
![bg opacity](./assets/gradient.jpg)

```js

// Hàm tính tổng chu vi của các hình chữ nhật
function sumPerimeterRectangles(rectangles) {
  const result = rectangles.reduce((total, rectangle) => {
    return total + calcPerimeterRectangle(rectangle.width, rectangle.height)
  }, 0);

  return result
}

// Các hình chữ nhật
const rectangles: Rectangle[] = [
  { width: 4, height: 5 },
  { width: 2, height: 3 },
  { width: 6, height: 7 }
];

// Tính tổng chu vi của các hình chữ nhật
const totalPerimeter: number = sumPerimeterRectangles(rectangles);
console.log("Total perimeter:", totalPerimeter);
```
<!--
Chúng ta định nghĩa một interface `Rectangle` để mô tả hình chữ nhật với các thuộc tính width và height. 

Hàm `calcPerimeterRectangle` sử dụng một đối tượng hình chữ nhật và tính toán chu vi dựa trên các thuộc tính width và height. 

Điều này cho phép chúng ta dễ dàng mở rộng tính năng của hàm này bằng cách chỉ định thêm thuộc tính trong interface `Rectangle` mà không cần phải sửa đổi hàm tính chu vi.
-->
---

![bg opacity](./assets/gradient.jpg)

### 3. Nguyên tắc thay thế Liskov (Liskov Substitution Principle - LSP)

- Nguyên tắc này nói rằng các thực thể (đối tượng, class con, function con)  có thể thay thế bằng các thực thể của lớp cha mà không làm thay đổi tính đúng đắn của chương trình.

<style scoped> { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)

```js
// Giao diện cho các hình học có khả năng tính diện tích
interface Shape {
  calculateArea(): number;
}

// Lớp hình chữ nhật (Rectangle) triển khai giao diện Shape
class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}

  calculateArea(): number {
    return this.width * this.height;
  }
}
```

---
![bg opacity](./assets/gradient.jpg)

```js
// Lớp hình vuông (Square) cũng triển khai giao diện Shape
class Square implements Shape {
  constructor(private sideLength: number) {}

  calculateArea(): number {
    return Math.pow(this.sideLength, 2);
  }
}

// Hàm tính tổng diện tích của các hình
function sumAreas(shapes: Shape[]): number {
  let totalArea = 0;
  for (let shape of shapes) {
    totalArea += shape.calculateArea();
  }
  return totalArea;
}
```
---
![bg opacity](./assets/gradient.jpg)

```js
// Sử dụng các hình học
const rectangle = new Rectangle(4, 5);
const square = new Square(4);

// Tính tổng diện tích của các hình
const totalArea = sumAreas([rectangle, square]);
console.log("Total area:", totalArea);
```
<!--
Trong ví dụ này, `Rectangle` và `Square` đều triển khai giao diện `Shape`, và `sumAreas` chấp nhận một mảng các đối tượng `Shape`. 

Khi chúng ta truyền một đối tượng `Rectangle` hoặc `Square` vào `sumAreas`, chúng ta đang tuân thủ nguyên tắc Thay thế Liskov, vì cả hai loại hình này có thể thay thế cho nhau mà không làm thay đổi tính đúng đắn của chương trình.
-->
---
![bg opacity](./assets/gradient.jpg)

### 4. Nguyên tắc phân tách giao diện (Interface Segregation Principle - ISP)

- Không nên dồn hết các prop hoặc các method vào trong một interface mà nên tách riêng ra các interface nhất định, có thể giảm thiểu sự phụ thuộc không cần thiết và tăng tính linh hoạt của code.

<style scoped> { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)

```js
interface ActiveIngredient {
  code: string;
  name: string;
  note: string;
  status: boolean;
}

interface ActiveIngredientResponse extends ActiveIngredient{
  id: string;
  creationTime: string;
  creatorId: string;
  lastModificationTime: string;
  lastModifierId: string;
}
```
<!--
Ví dụ trên chúng ta có thể thấy rằng, interface ActiveIngredient được dùng để Create; Update data bao gồm các prop như code, name ...

Nhưng khi Get thì cần những prop như id, creationTime... để hiển thị lên giao diện thì interface ActiveIngredient lại không đáp ứng được. Nếu sửa interface ActiveIngredient thì lại phạm vào quy tắc số 2 là Nguyên tắc đóng mở

Và nếu có sửa được thì các function hoặc class cũ đã dùng interface ActiveIngredient cũng không dùng đến các prop mới như id, creationTime... để sử dụng

Vậy nên tách ra một interface mới để tránh ảnh hưởng tới các chức năng cũ đã sử dụng interface ActiveIngredient
-->
---

![bg opacity](./assets/gradient.jpg)

### 5. Nguyên tắc nghịch đảo phụ thuộc (Dependency Inversion Principle - DIP)

- Các module cấp cao không nên phụ thuộc vào các modules cấp thấp. Cả 2 nên phụ thuộc vào abstraction.

- Interface (abstraction) không nên phụ thuộc vào chi tiết, mà ngược lại. (Các class giao tiếp với nhau thông qua interface, không phải thông qua implementation).

<style scoped> { text-align: left; }</style>
---
![bg opacity](./assets/gradient.jpg)

Không áp dụng **Nguyên tắc nghịch đảo phụ thuộc**:
```js
// class OrderService phụ thuộc trực tiếp vào class PayPalPaymentService
class OrderService {
  constructor() {
    this.paymentService = new PayPalPaymentService();
  }

  processOrder(order: any) {
    // Xử lý đơn hàng và thanh toán bằng dịch vụ thanh toán PayPal
    this.paymentService.pay(order.totalAmount);
    // Các bước xử lý khác...
  }
}

// class PayPalPaymentService
class PayPalPaymentService {
  pay(amount: number) {
    console.log(`Paying $${amount} via PayPal`);
    // Gọi API PayPal để xử lý thanh toán
  }
}
```
<!--
 `OrderService` phụ thuộc trực tiếp vào `PayPalPaymentService`, điều này là không tốt vì nó làm cho `OrderService` cứng chỉ có một dịch vụ thanh toán cụ thể. 
 
 Khi chúng ta muốn thay đổi hoặc mở rộng hỗ trợ cho các dịch vụ thanh toán mới, chúng ta sẽ phải sửa đổi trực tiếp `OrderService`, điều này không tuân thủ nguyên tắc DIP.
-->

<style scoped>p { text-align: left; }</style>
---
![bg opacity](./assets/gradient.jpg)

Áp dụng **Nguyên tắc nghịch đảo phụ thuộc**:
```js
// Định nghĩa một interface trừu tượng cho các dịch vụ thanh toán
interface PaymentService {
  pay(amount: number): void;
}

// PayPalPaymentService triển khai interface PaymentService
class PayPalPaymentService implements PaymentService {
  pay(amount: number) {
    console.log(`Paying $${amount} via PayPal`);
    // Gọi API PayPal để xử lý thanh toán
  }
}
```
<style scoped>p { text-align: left; }</style>
---

![bg opacity](./assets/gradient.jpg)

```js
// GooglePayPaymentService triển khai interface PaymentService
class GooglePayPaymentService implements PaymentService {
  pay(amount: number) {
    console.log(`Paying $${amount} via Google Pay`);
    // Gọi API Google Pay để xử lý thanh toán
  }
}

// class OrderService không phụ thuộc trực tiếp vào các dịch vụ thanh toán cụ thể
class OrderService {
  constructor(private paymentService: PaymentService) {}

  processOrder(order: any) {
    // Xử lý đơn hàng và thanh toán bằng dịch vụ thanh toán được cung cấp
    this.paymentService.pay(order.totalAmount);
    // Các bước xử lý khác...
  }
}
```
---

![bg opacity](./assets/gradient.jpg)

```js
// Sử dụng OrderService với PayPalPaymentService
const paypalService = new PayPalPaymentService();
const orderServiceWithPayPal = new OrderService(paypalService);
orderServiceWithPayPal.processOrder({ totalAmount: 100 });

// Sử dụng OrderService với GooglePayPaymentService
const googlePayService = new GooglePayPaymentService();
const orderServiceWithGooglePay = new OrderService(googlePayService);
orderServiceWithGooglePay.processOrder({ totalAmount: 200 });
```
<!--
Trong ví dụ này, `OrderService` không phụ thuộc trực tiếp vào các dịch vụ thanh toán cụ thể như `PayPalPaymentService` hoặc `GooglePayPaymentService`.

Thay vào đó, nó phụ thuộc vào một giao diện trừu tượng `PaymentService`. Điều này giúp chúng ta dễ dàng thay đổi hoặc mở rộng hỗ trợ cho các dịch vụ thanh toán mới mà không cần phải sửa đổi `OrderService`,
-->
---
![bg opacity](./assets/gradient.jpg)

### 🔗 LINK THAM KHẢO

- [Tôi đi code dạo](https://toidicodedao.com/2015/03/24/solid-la-gi-ap-dung-cac-nguyen-ly-solid-de-tro-thanh-lap-trinh-vien-code-cung/)

---

![bg opacity](./assets/gradient.jpg)

### ❓ Q&A 

---

![bg opacity](./assets/gradient.jpg)

### 📖 THANK YOU 
