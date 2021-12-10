### 关于字符串操作
1. 先reserve()，再使用[]操作字符串并不会改变其length，vector<>同理

    operator[] does not resize the string. Use resize to set the string to a specific size, or push_back or operator+= to append characters.

   但是resize()的参数必须准确，否则会计入未赋值区域；所以推荐使用operator+=/append()
