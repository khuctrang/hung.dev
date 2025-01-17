---
template: post
title: Hãy nói về Referrer-Policy - Bạn có đang bị dòm ngó khi duyệt web không?
slug: referrer-policy
draft: false
date: 2021-05-27T17:24:44.755Z
description: Tìm hiểu về Referrer-Policy và cách nó bảo vệ bạn khi truy cập internet
category: "Blog"
tags:
  - referer
  - referrer-policy
  - internet tracking
  - http header
---
### Dẫn nhập

Như chúng ta đã biết HTTP Header `Referer` thể hiện rằng bạn đến từ đâu khi truy cập một trang web. Giả dụ rằng bạn đang lướt [www.facebook.com](http://www.facebook.com), bỗng nhìn thấy một chiếc link đến cửa hàng bán chiếc áo đang sale. Bạn click vào đó để xem chiếc áo vì tò mò, có thể bạn cũng sẽ mua chiếc áo đó. Một điều không bất ngờ lắm đó là ngay thời điểm bạn click vào chiếc link, cửa hàng quần áo kia sẽ biết được bạn đến với trang web của họ từ facebook. Hơn nữa, họ có thể đong đếm được tỉ lệ số người mua hàng đến từ facebook so với các kênh marketing khác là bao nhiêu. Từ đó có thể có những sự điều chỉnh phù hợp cho chiến dịch quảng cáo. Cách hoạt động thì tương đối đơn giản, mỗi khi người dùng ấn vào một đường link, một header `referer` sẽ được đính kèm.

![clothes](https://images.unsplash.com/photo-1489987707025-afc232f7ea0f?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2100&q=80 "Lướt web thấy áo đẹp")

Hình dưới đây mô tả việc referer được gửi đi khi bạn đọc báo [dantri.com.vn](http://dantri.com.vn)

![](/media/referer-dantri.jpg)

Tuy nhiên, hãy xem xét đến một ví dụ khác, đó là Google Docs. Như chúng ta vẫn hay sử dụng Google Docs để chia sẻ tài liệu với nhau. Đơn giản nhất đó là qua hai bước:

1. Share quyền của văn bản đó là "Anyone with link can view/edit"
2. Gửi link cho người khác

Mỗi link của một văn bản đều có dạng [https://docs.google.com/document/d/**some-random-id**/edit](https://docs.google.com/document/d/some-random-id/edit). Về cơ bản `some-random-id` đủ phức tạp để không ai có thể tự mò ra được, ngoại trừ việc được chia sẻ. Cho nên về cơ bản việc chia sẻ link cho nhau là tương đối an toàn.

Lấy ví dụ với văn bản [https://docs.google.com/document/d/1QHmwfKoekal9HJDu-_3zol5Ht2TGtfiSumHWVD9LRgo/edit](https://docs.google.com/document/d/1QHmwfKoekal9HJDu-_3zol5Ht2TGtfiSumHWVD9LRgo/edit). Trong văn bản này có chứa đường link của báo [Dân Trí](https://dantri.com.vn/) và báo [VnExpress](https://vnexpress.net/), đồng thời cả thông tin tài khoản ngân hàng của mình. Nếu bạn ấn vào đường link trong file bên trên, liệu rằng request đến báo Dân Trí và VnExpress có được đính kèm header dưới đây hay không?

> Referer: https://docs.google.com/document/d/1QHmwfKoekal9HJDu-_3zol5Ht2TGtfiSumHWVD9LRgo/edit 

![](/media/screenshot-2021-05-28-at-00.34.05.png)


Nếu câu trả lời là có, vậy nghĩa là người quản trị web của Dân Trí và VnExpress có thể biết được link của tài liệu của mình, đồng nghĩa với việc họ biết được thông tin tài khoản ngân hàng của mình, nếu mình lại chia sẻ tài liệu này với quyền ***Anyone with link can edit***, thì mọi chuyện còn tệ hơn, người đó có thể đổi mật khẩu trong tài liệu này khiến mình lần tới không thể đăng nhập được.

Nghe có vẻ bất ngờ, bạn có chột dạ rằng mình có thể bị lộ bao nhiêu tài liệu từ trước đến nay có phải không? Vậy bạn hãy dành ra 2 phút để tự mình kiểm chứng nhé.

![](/media/eternity.jpg)

OK. Nếu bạn đã check, thì có thể sẽ thấy ngạc nhiên vì đúng là có header `Referer` gửi đi khi click vào trang báo thật, nhưng giá trị của nó chỉ là <https://www.google.com/> chứ hoàn toàn không có giá trị của `some-random-id`. 

![](/media/dantri-header.png)

Vậy nghĩa là bạn **vẫn an toàn**. Nói cách khác, nếu bạn ấn vào một đường link trong [Google Docs](https://docs.google.com/), thì người quản trị trang web đó chỉ biết bạn vào trang web của họ từ trang web của google, chứ không thể biết chính xác là link nào. Vậy tại sao lại thế?

### Referrer-Policy

Cứu tinh của chúng ta chính là **Referrer-Policy**. Trong ví dụ bên trên, hiểu nôm na là, google docs không cho phép gắn link đầy đủ vào header referer, mà nó chỉ gắn duy nhất origin, có nghĩa là `some-random-id` không được tiết lộ cho Dân Trí và VnExpress, hay bất cứ trang web nào khác. Vậy chính xác Referrer-Policy là gì?

> The **Referrer-Policy** [HTTP header](https://developer.mozilla.org/en-US/docs/Glossary/HTTP_header) controls how much [referrer information](https://developer.mozilla.org/en-US/docs/Web/Security/Referer_header:_privacy_and_security_concerns) (sent via the [Referer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) header) should be included with requests.

Referrer-Policy là một HTTP Header điều khiển việc mình gửi thông tin referrer đi. Tuỳ thuộc vào nhu cầu mình muốn gửi đi bằng nào thông tin về referrer, hay bảo vệ thông tin đến mức nào mà có thể lựa chọn rule cho phù hợp. Chúng ta hãy cùng đi qua một vài các rule của Referrer-Policy nhé.

#### Một vài rule của Referrer-Policy

**unsafe-url:** Luôn gửi referrer

**no-referrer:** Không gửi referrer

**no-referrer-when-downgrade:** Không gửi referrer khi điểm đến có protocol có tính chất bảo mật kém hơn (HTTPS => HTTP, HTTPS => files)

**origin:** Chỉ gửi origin. Ví dụ bạn đang ở <https://example.com/page.html> thì khi đến một trang khác, giá trị của referrer sẽ là <https://example.com/>

**same-origin:** Gửi referrer full url khi cùng origin, nếu khác thì chỉ send origin\
Ví dụ: Nếu đang ở <https://example.com/page.html>, sau đó sang trang <https://example.com/about.html>, thì referrer sẽ là <https://example.com/page.html>. \
Còn nếu đang ở <https://example.com/page.html> mà sang <https://google.com>, thì referrer sẽ chỉ là https://example.com/

Các rule khác của Referrer-Policy các bạn có thể tham khảo chi tiết tại [đây](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy#directives)

### Set rule bằng HTML

Ngoài HTTP Header, chúng ta hoàn toàn có thể set Referrer-Policy [sử dụng HTML](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy#integration_with_html)

```html
<meta name="referrer" content="origin">
```

### New default

Từ tháng 11 năm 2020, giá trị mặc định của Referrer-Policy là `strict-origin-when-cross-origin` trên nhiều trình duyệt (e.g: Chrome 85, Firefox 87,…), mô tả của nó như sau:

> Send the origin, path, and querystring when performing a same-origin request. For cross-origin requests send the origin (only) when the protocol security level stays same (HTTPS→HTTPS). Don't send the [Referer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) header to less secure destinations (HTTPS→HTTP).

Trước đó giá trị mặc định của Referrer-Policy là `no-referrer-when-downgrade` . Sự thay đổi này có nghĩa là nếu bạn click vào một link trên trang web, trình duyệt sẽ giấu đi thông tin bạn đến chính xác từ url nào. Ví dụ bạn đang ở [https://docs.google.com/document/d/1QHmwfKoekal9HJDu-_3zol5Ht2TGtfiSumHWVD9LRgo/edit](https://docs.google.com/document/d/1QHmwfKoekal9HJDu-_3zol5Ht2TGtfiSumHWVD9LRgo/edit) rồi ấn vào [Dân Trí](https://dantri.com.vn/). Dân Trí chỉ biết rằng bạn đến từ google, chứ không hề biết bạn đến từ văn bản có chứa cả tài khoản ngân hàng của bạn. Việc này vô hình trung đã đảm bảo an toàn và tính riêng tư của bạn hơn khi sử dụng internet. Nhưng đối với các bạn web developer cũng nên chú ý xem trong hệ thống của bạn đang có feature nào dựa vào thông tin referrer hay không, nếu có thì từ Chrome 85, Firefox 87 đổ đi nó có còn hoạt động đúng hay không, nếu không thì hãy lên kế hoạch chỉnh sửa cho phù hợp nhé.

### Fun fact:

Header `Referer` [viết sai chính tả](https://stackoverflow.com/questions/3087626/was-the-misspelling-of-the-http-field-name-referer-intentional), đúng chính tả phải là `Referrer`, nhưng đến khi nhiều người dùng quá rồi thì không ai dám sửa cả. Sợ hỏng cả hệ thống internet vốn đã phức tạp. Đây là ví dụ rất đặc trưng cho cách mà toàn bộ hệ thống số trên toàn cầu được xây dựng và vận hành 😅. Và là mảnh đất màu mỡ cho các hacker trên toàn thế giới để khai thác.

### Kết luận

Qua bài viết này, chúng ta có thể học được vài điều:

* Hiểu được Referrer-Policy là gì?
* Không nên lưu mật khẩu vào google docs, Note,… 😂
* Nếu tài liệu có thông tin nhạy cảm nên hạn chế share kiểu **Anyone with link can view/edit**
* Nếu làm sai cái gì nên sửa càng sớm càng tốt, đừng để đến khi nó gây ảnh hưởng lớn quá không ai dám sửa 🙈

Chúc các bạn khoẻ mạnh an toàn qua mùa dịch bệnh và cùng nhau học hỏi để tiến bộ mỗi ngày nhé.