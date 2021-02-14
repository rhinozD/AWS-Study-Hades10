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
<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/OAuth2/4.PNG" alt="OAuth2.0 overview flow">
</p>
