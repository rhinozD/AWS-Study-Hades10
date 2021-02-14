## HISTORY
 Trong quá khứ(tầm hơn 10 năm trước), thì chỉ có các identity use cases sau:
 - Simple login - sử dụng database lưu thông tin identity, login bằng forms hoặc cookies.
 - Single sign-on across sites - sử dụng SAML
([What is SAML and How Does it Work?](https://www.varonis.com/blog/what-is-saml/))
([Security Assertion Markup Language](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language))

Vấn đề phát sinh:
 - Tuy nhiên tại thời điểm đó, vẫn chưa có một cái standard nào sử dụng vào case Mobile app login.
 - Cũng như chưa có stadard nào định nghĩa cho việc uỷ quyền(Delegated authorization).

## The delegated authorization problem 
### Làm sao có thể cho website access data của mình?
- cung cấp tài khoản/mật khẩu để uỷ quyền cho website đăng nhập? -> absolutely bad way!!
<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/1.PNG" alt="Bad way 1">
</p>
<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/1.PNG" alt="Bad way 2">
</p>

### Delegated authorization with OAuth2.0
- Giả sử: 
  - muốn connect yelp account và gmail account.
  - Yelp chỉ có quyền access đến thông tin contacts(read-only, không có quyền write hay quyền access những thông tin khác)
<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/3.PNG" alt="OAuth2.0 Idea">
</p>

**-> Có thể thực hiện với OAuth2.0**
1. User click "Connect with Google"
2. User nhập thông tin đăng nhập google.
3. User có thể click vào yes để cho phép Yelp access thông tin của mình. Có thể click No để Deny.
4. Trong trường hợp User click Yes ở bước 3, yelp sẽ nhận được được access token từ google.
5. Sử dụng access token để access và lấy thông tin contacts.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/4.PNG" alt="OAuth2.0 overview flow">
</p>

## OAuth2.0
### Các thuật ngữ
- Resource owner: User( thằng ngồi sau bàn phím bấm yes, no)
- Client: Application cần được cấp quyền access (Yelp)
- Authorization server: Server hoặc system chứa users identity(- chứa user account) (accounts.google.com)
- Resource server: Server có data mà client muốn lấy(có thể là contacts API của google). Nhiều khi Authorization server và Resource server được gộp làm một server.
- Authorization grant: Các phương thức Author dùng để lấy được access token.
- Access token: Được dùng để gửi để resource server và lấy data cần lấy.

### OAuth2.0 authorization code flow

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/5.PNG" alt="authorization code flow">
</p>

### Thêm thuật ngữ
- Scope: String, giới hạn quyền access.
- Consent: 
