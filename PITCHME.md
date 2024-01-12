---
marp: true
title: Workshop ASP
description: Giới thiệu về Browser Caching
theme: uncover
transition: fade
paginate: true
_paginate: false
---

![bg opacity](./assets/gradient.jpg)

# Tận dụng Caching để tối ưu hóa hiệu suất trình duyệt

<!-- This is presenter note. You can write down notes through HTML comment. -->

---

![bg opacity](./assets/gradient.jpg)

### Mục tiêu của workshop:

Hiểu rõ về cơ chế caching trong trình duyệt và cách sử dụng nó để tối ưu hiệu suất ứng dụng web.

<style scoped>p { text-align: left; }</style>

---

![bg opacity](./assets/gradient.jpg)

### Tại sao **[Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)** quan trọng?

- Các trình duyệt thường sẽ lưu lại các bản copy của các static asset ở local để giảm thời gian tải và tối thiểu hóa lượng dữ liệu phải truyền tải, việc này gọi là caching.

- Việc caching sẽ giúp giảm thời gian tải, cùng với đó việc không tải những dữ liệu không cần thiết cũng giúp giảm lưu lượng phải truyền tải.

---

![bg opacity](./assets/gradient.jpg)

### Có 2 dạng Caching

1. Caching Tại Mức Điều Khiển (Client-Side)
2. Caching Tại Mức Proxy Server (Server-Side)

---

![bg opacity](./assets/gradient.jpg)

### Caching tại mức điều khiển (Client-Side)

- Quá trình lưu trữ và tái sử dụng tài nguyên trên máy khách (trình duyệt) của người dùng

- Mục tiêu của caching này là giảm thời gian tải trang web và tối ưu hóa trải nghiệm người dùng bằng cách giảm lượng dữ liệu cần phải tải lại từ máy chủ.

<!-- ## **[GitHub Pages](https://github.com/pages)** -->

<!-- #### Ready to write & host your deck! -->

<!-- [![Use this as template h:1.5em](https://img.shields.io/badge/-Use%20this%20as%20template-brightgreen?style=for-the-badge&logo=github)](https://github.com/yhatt/marp-cli-example/generate) -->

---

![bg opacity](./assets/gradient.jpg)

### Một số khái niệm trong Caching Client-Side

1. **[Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)**: Hiển thị một số giá trị như max-age, no-cache, no-store,... được sử dụng để định rõ cách trình duyệt nên lưu trữ và sử dụng tài nguyên.

```
Cache-Control: max-age=3600
```

2. **[Expires](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires)**: chỉ định một thời điểm cụ thể trong tương lai khi tài nguyên sẽ hết hạn.

```
Expires: Thu, 01 Jan 2023 00:00:00 GMT
```

<!-- ![bg right 60%](https://icongr.am/simple/netlify.svg?colored)

## **[Netlify](https://www.netlify.com/)**

#### Ready to write & host your deck!

[![Deploy to Netlify h:1.5em](./assets/netlify-deploy-button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/yhatt/marp-cli-example) -->

---

![bg opacity](./assets/gradient.jpg)

3. **[LocalStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)** và **[SessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)**: là hai API của HTML5 được sử dụng để lưu trữ dữ liệu trên máy khách.

- Dữ liệu được lưu trong **LocalStorage** có thể tồn tại mãi mãi, trong khi dữ liệu trong **SessionStorage** chỉ tồn tại trong phiên làm việc của trình duyệt.

4. **[Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)**: là một công nghệ mạnh mẽ cho caching và xử lý sự kiện offline.

- Cho phép triển khai các chiến lược caching phức tạp và quản lý tài nguyên nằm ngoài khả năng của trình duyệt chính.

<!-- ![bg right 60%](https://icongr.am/simple/zeit.svg)

## **[Vercel](https://vercel.com/)**

#### Ready to write & host your deck!

[![Deploy to Vercel h:1.5em](https://vercel.com/button)](https://vercel.com/import/project?template=https://github.com/yhatt/marp-cli-example) -->

---

![bg opacity](./assets/gradient.jpg)

### Caching tại mức **[Proxy Server](https://developer.mozilla.org/en-US/docs/Glossary/Proxy_server)** (Server-Side)

- Là quá trình lưu trữ tài nguyên không chỉ trên trình duyệt client mà còn trên các proxy server và máy chủ. Điều này giúp giảm tải cho máy chủ và tăng tốc độ tải trang web cho nhiều người dùng bằng cách giảm lượng dữ liệu phải đi qua mạng.

<!-- ### fit :ok_hand: -->

---

![bg opacity](./assets/gradient.jpg)

1. **[Cache-Control Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)**
- **Ưu điểm**:
  - Cung cấp kiểm soát linh hoạt: Cache-Control là một header rất linh hoạt với nhiều chỉ thị như max-age, no-cache, no-store, public, private,...
  - Cho phép định rõ thời gian tối đa mà tài nguyên có thể được lưu trữ trên máy khách và các proxy server.
- **Hạn Chế**:
  - Đôi khi việc cấu hình Cache-Control có thể đòi hỏi sự cân nhắc kỹ lưỡng để đảm bảo hiểu quả và đồng thời giữ cho trang web luôn cập nhật.

---

![bg opacity](./assets/gradient.jpg)

2. **[Expires Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires)**

- **Ưu điểm**:

  - Xác định thời điểm cụ thể khi tài nguyên sẽ hết hạn.
  - Dễ hiểu và triển khai.

- **Hạn Chế**:

  - Thường không được ưa chuộng so với Cache-Control do thiếu sự linh hoạt.
  - Yêu cầu máy chủ đồng bộ hóa chính xác với thời gian hết hạn.

---

![bg opacity](./assets/gradient.jpg)

3. **[ETag Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)**

- **Ưu điểm**:

  - Dùng để kiểm tra xem tài nguyên có thay đổi hay không.
  - Giúp máy chủ tránh việc gửi lại toàn bộ tài nguyên khi nó không thay đổi.

- **Hạn Chế**:

  - Tăng kích thước gói tin và có thể tạo ra overhead nếu không được sử dụng đúng cách.

---

![bg opacity](./assets/gradient.jpg)

4. **[Vary Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary)**

- **Ưu điểm**:

  - Xác định các yếu tố nào sẽ được xem xét khi kiểm tra xem tài nguyên đã được lưu trữ chưa (ví dụ: Vary: User-Agent).

- **Hạn Chế**:

  - Cần phải được sử dụng cẩn thận để tránh các vấn đề về caching không mong muốn.

---

![bg opacity](./assets/gradient.jpg)

5. **[Pragma Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Pragma)**

- **Ưu điểm**:

  - Chủ yếu được sử dụng như một phương tiện tương thích với các phiên bản HTTP cũ hơn.

- **Hạn Chế**:

  - Đã lạc lõng trong các phiên bản gần đây và thường được thay thế bằng Cache-Control.

---

![bg opacity](./assets/gradient.jpg)

### So sánh giữa **Client-Side Caching** và **Server-Side Caching**

**Client-Side Caching**

- **Điểm mạnh**:
  - **Tốc độ truy cập nhanh**: Tài nguyên được lưu trữ trực tiếp trên trình duyệt client, giúp giảm thời gian tải trang cho người dùng khi họ truy cập lại trang web.
  - **Giảm tải cho máy chủ**: Giảm áp lực lên máy chủ do tài nguyên được lưu trữ và sử dụng từ bộ nhớ cache của trình duyệt client.

<style scoped>p { text-align: left; }</style>

---

![bg opacity](./assets/gradient.jpg)

- **Hạn chế**:
  - **Dung lượng hạn chế**: Bộ nhớ cache trên trình duyệt client có thể bị giới hạn và có thể bị xóa bất cứ lúc nào, đặc biệt là khi người dùng xóa lịch sử trình duyệt.
  - **Không kiểm soát tuyệt đối**: Không hoàn toàn kiểm soát được bởi máy chủ, điều này có thể dẫn đến việc sử dụng phiên bản cũ của tài nguyên khi đóng bộ nhớ cache.

---

![bg opacity](./assets/gradient.jpg)

**Server-Side Caching**

- **Điểm mạnh**:
  - **Kiểm soát cao**: Máy chủ có kiểm soát đầy đủ về cách tài nguyên được lưu trữ và tái sử dụng.
  - **Áp dụng đối với tất cả người dùng**: Caching ở mức server-side áp dụng cho tất cả người dùng, giúp giảm thời gian tải trang cho mọi người dùng truy cập.

<style scoped>p { text-align: left; }</style>

---

![bg opacity](./assets/gradient.jpg)

- **Hạn chế**:
  - **Tăng tải cho máy chủ**: Nếu không được quản lý cẩn thận, caching tại mức server-side có thể tăng gánh nặng cho máy chủ khi phải xử lý nhiều yêu cầu caching.
  - **Không hiệu quả đối với dữ liệu động**: Caching server-side thường không hiệu quả đối với dữ liệu động, đặc biệt là nếu dữ liệu thay đổi thường xuyên.

---

![bg opacity](./assets/gradient.jpg)

### REFERENCES

- Unordered list can use asterisks
- Unordered list can use asterisks
- Unordered list can use asterisks

---

![bg opacity](./assets/gradient.jpg)

### Q&A

---

![bg opacity](./assets/gradient.jpg)

### THANK YOU
