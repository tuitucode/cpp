---
published: true
layout: post
title: Cơ bản về kế thừa
categories: oop
img: bai26.png
lesson: 16
excerpt_separator: <!--more-->
---
Ở bài trước chúng ta đã tìm hiểu về hợp thành (composition) có thể giúp chúng ta xây dựng 1 cấu trúc phức tạp dựa vào những bộ phận đơn giản hơn bên trong, đây là 1 trong 2 cách mà C++ hỗ trợ chúng ta để xây dựng nên 1 mô hình phức tạp và cách thứ 2 chính là dùng kế thừa (inheritance).<!--more-->
## Kế thừa...thừa kế
Kế thừa là một đặc điểm của ngôn ngữ dùng để biểu diễn mối quan hệ đặc biệt hóa – tổng quát hóa giữa các lớp (quan hệ "is-a"). Các lớp được trừu tượng hóa và được tổ chức thành một sơ đồ phân cấp lớp.

Trước tiên, các bạn hãy nhìn ví dụ thông qua ảnh bên dưới
![Kế thừa - TuiTuCode](https://1.bp.blogspot.com/-xK39l74JKN8/XS2l8yl3z0I/AAAAAAAAAkU/2R2HGoO9JOEhTnp3jXYI3KBVUrmVwZwuACLcBGAs/s1600/Capture.PNG)

Ta có thể thấy 1 cây sơ đồ trong hình với ý nghĩa phân loại động vật, tuy động vật trên cạn và động vật dưới nước có những đặc tính khác nhau, mỗi loại đều có những đối tượng cụ thể (trên cạn: mèo, ngựa.. dưới nước: cá, mực,..) với những đặc điểm riêng nhưng chúng đều được xếp loại chung thành lớp động vật, đều thừa hưởng mọi đặc tính từ động vật (như có tên, có thể thở,..).

Sơ đồ trên còn được gọi là sơ đồ phân cấp: động vật dưới nước là "cha" của loài cá đồng thời lại là "con" của động vật.

Qua ví dụ trên các bạn chắc cũng đã nắm được phần nào những đặc điểm của kế thừa rồi đúng không? Chúng ta có thể tổng hợp thành 2 từ xúc tích: thừa hưởng đặc điểm và phân cấp. Bây giờ chúng ta sẽ đi vào tìm hiểu kế thừa trong C++ sẽ được thực hiện như thế nào nhé!

## Kế thừa trong C++
Đầu tiên chúng ta cùng đi đến mô hình cài đặt kế thừa trong C++ thông qua class như sau:
{% highlight cpp %}
	class Base {
    	// Thành phần của lớp cơ sở
    };
    
    class Derived : [private/protected/public] Base {
    	// Thành phần riêng của lớp dẫn xuất
    };
{% endhighlight %}

Phân tích: Chúng ta sẽ dùng toán tử ``:`` để tiến hành kế thừa lớp Base theo 1 trong 3 cách kế thừa: private, protected và public (phạm vi truy xuất trong kế thừa).

### Phạm vi truy xuất
Mỗi loại kế thừa đều có những đặc điểm khác nhau. Chúng ta có 2 loại truy xuất: truy xuất theo chiều ngang và truy xuất theo chiêu dọc.

**Truy xuất theo chiều dọc:** nói về quyền truy xuất vào các thuộc tính và phương thức (với các kiểu private, protected và public) của lớp từ những vị trí khác nhau (như bên trong lớp, ở lớp con và bên ngoài lớp). Cùng xem bảng bên dưới để hiểu rõ:

<table class="table">
<thead>
<tr class="header">
<th>Truy cập</th>
<th>private</th>
<th>protected</th>
<th>public</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Bên trong lớp</td>
<td markdown="span">Có</td>
<td markdown="span">Có</td>
<td markdown="span">Có</td>
</tr>
<tr>
<td markdown="span">Lớp kế thừa</td>
<td markdown="span">Không</td>
<td markdown="span">Có</td>
<td markdown="span">Có</td>
</tr>
<tr>
<td markdown="span">Bên ngoài lớp</td>
<td markdown="span">Không</td>
<td markdown="span">Không</td>
<td markdown="span">Có</td>
</tr>
</tbody>
</table>

_*Giải thích:_  Có: được phép truy cập, Không: không được phép truy cập. Ơ mức truy cập ngoài lớp không bao gồm hàm bạn, lớp bạn (friend function, friend class) vì đặc tính của nó là được truy cập các thuộc tính private (cũng như protected và public).

_*Lưu ý:_ phạm vi truy xuất trong bản là của các thuộc tính và phương thức trong lớp chứ không phải phạm vi truy xuất phía sau toán tử ``:`` khi lớp con kế thừa lớp cha. 

**Truy xuất theo chiều ngang:** nói về sự truy xuất các thành phần của lớp cha thông qua lớp con từ thế giới bên ngoài, đây chính là những phạm vi đươc lớp con sử dụng khi kế thừa lớp cha.
{% highlight cpp %}
	class Derived : [private/protected/public] Base {
    	// Thành phần riêng của lớp dẫn xuất
    };
{% endhighlight %}
Chúng ta sẽ xem ví dụ sau:
{% highlight cpp %}
    #include <iostream>
    using namespace std;
     
    class A {
    	private:
    	int aPriv;
    	protected:
    	int aProt;
    	public:
    	int aPub;
    };
     
    class B : private A {
    	/* 
    	* private aProt;
    	* private aPub;
    	*/
    };
     
    class C : protected A {
    	/* 
    	* protected aProt;
    	* protected aPub;
    	*/
    };
     
    class D : public A {
    	/* 
    	* protected aProt;
    	* public aPub;
    	*/
    };
     
    int main() {
    	// your code goes here
    	return 0;
    }
{% endhighlight %}
Những dòng comment chính là cách lớp con kế thừa những thuộc tính của lớp cha và chuyển phạm vi truy xuất về tương ứng với kiểu kế thừa, chi tiết:
  - Lớp B kế thừa private lớp A: 2 thuộc tính public và protected của lớp A (lớp cha) sẽ trở thành thuộc tính private ở lớp B.
  - Lớp C kế thừa protected lớp A: 2 thuộc tính public và protected của lớp A (lớp cha) sẽ trở thành thuộc tính protected ở lớp C.
  - Lớp C kế thừa protected lớp A: 2 thuộc tính public và protected của lớp A (lớp cha) sẽ được giữ nguyên (vẫn là public và protected) ở lớp C.

Tại sao lại không đề cập đến thuộc tính private (aPriv) của lớp A? Theo như truy xuất theo chiều dọc (bảng phía trên) ta thấy lớp kế thừa (lớp con) không có quyền truy cập vào thuộc tính private của lớp cha.
  
Sau khi đã chuyển đổi phạm vi tương ứng, các thuộc tính từ lớp cha bây giờ đã trở thành thuộc tính của lớp con (với các kiểu truy xuất mới) và ở bên ngoài sẽ truy xuất dựa trên sự thay đổi này.
### Đơn kế thừa
Đơn kế thừa là loại kế thừa dựa trên mối quan hệ 1 - 1. VD: 1 sinh viên cũng là 1 con người.

Trong ví dụ quan hệ 1 - 1, chúng ta có thể nói chi tiết hơn: 1 sinh viên là 1 con người nhưng có thêm 1 số đặc điểm riêng biệt (đi học, làm bài tập về nhà,...). Như vậy ta sẽ biểu diễn lớp SinhVien kế thừa lớp ConNguoi như sau:
{% highlight cpp %}
    #include <iostream>
    #include <string>
    using namespace std;
     
    class ConNguoi {
    	private:
    	//Properties
    	string CMND;
    	string HoTen;
    	string Tuoi;
    	public:
    	//Constructors
    	ConNguoi();
    	ConNguoi(string id, string hoten, string tuoi) {
    		CMND = id;
    		HoTen = hoten;
    		Tuoi = tuoi;
    	}
    	//Methods
    	void An() {
    		cout << "Dang an..." << endl;
    	};
    	void Ngu() {
    		cout << "Dang ngu..z.z" << endl;
    	};
    	void SinhHoat() {
    		cout << "Dang sinh hoat..." << endl;
    	};
     
    	void Xuat() {
    		cout << "CMND: "<< CMND << endl;
    		cout << "Ho ten: " << HoTen << endl;
    		cout << "Tuoi: " << Tuoi << endl;
    	}
     
    };
     
    class SinhVien : public ConNguoi  {
    	private:
    	//Properties
    	string MSSV;
    	string Lop;
    	public:
    	//Constructors
    	SinhVien();
    	SinhVien(string mssv, string lop,string id, string hoten, string tuoi) : ConNguoi(id, hoten, tuoi) {
    		MSSV = mssv;
    		Lop = lop;
    	}
    	//Methods
    	void Hoc() {
    		cout << "Dang hoc..." << endl;
    	}
    	void LamBaiTap() {
    		cout << "Lam bai tap..." << endl;
    	}
    	void Xuat() {
    		ConNguoi::Xuat();
    		cout << "MSSV: " << MSSV << endl;
    		cout << "Lop: " << Lop << endl;
    	}
    };
     
    int main() {
    	// your code goes here
    	SinhVien sv1("001","OOP","1189","Nguyen Van A","21");
     
    	sv1.Xuat();
  		sv1.An();
    	return 0;
    }
{% endhighlight %}
Kết quả chương trình:
{% highlight cpp %}
	CMND: 1189
	Ho ten: Nguyen Van A
	Tuoi: 21
	MSSV: 001
	Lop: OOP
	Dang an...
{% endhighlight %}
Trong đơn kế thừa, lớp SinhVien được kế thừa toàn bộ thuộc tính (CMND, HoTen, Tuoi) và các phương thức (An, Ngu, SinhHoat) của lớp ConNguoi nhưng riêng constructor thì không.
### Định nghĩa lại phương thức ở lớp con
Trong đoạn ví dụ trên chúng ta thấy ở cả lớp cha (ConNguoi) và lớp con (SinhVien) đều có phương thức giống tên nhau là phương thức **Xuat** cùng làm nhiệm vụ xuất thông tin các thuộc tính của lớp, đây là cách chúng ta định nghĩa lại phương thức đã có ở lớp cha (lúc này khi cần gọi phương thức đã có ở lớp cha chúng ta sẽ dùng toán tử phạm vi ``::``).
### Cách gọi constructor
Một vấn đề khác trong kế thừa là cách gọi constructor, xem tiếp ví dụ dưới đây:
{% highlight cpp %}
    #include <iostream>
    using namespace std;
     
    class Base {
    	private:
    	int value;
    	public:
    	Base() {
    		cout << "call Base constructor..." << endl;
    	}
    };
     
     
    class Derived : public Base {
    	public:
    	Derived() {
    		cout << "call Derived constructor..." << endl;
    	}
    };
     
    int main() {
    	// your code goes here
    	Derived test;
    	return 0;
    }
{% endhighlight %}
Kết quả chương trình:
{% highlight cpp %}
	call Base constructor...
	call Derived constructor...
{% endhighlight %}
Nhận thấy trình biên dịch đã gọi constructor của lớp cha rồi mới đến lớp con, đây là cách gọi constructor khi chúng ta sử dụng kế thừa, tương tự nếu chương trình của bạn có nhiều dẫn xuất (B kế thừa A, C kế thừa B,..) thì constructor sẽ được gọi từ lớp cha trước rồi đến lớp con.
### Cách gọi destructor
Ngược lại với gọi constructor, destructor sẽ được gọi từ lớp con rồi mới đến lớp cha.
## Tổng kết
Vậy là chúng ta đã tìm hiểu 1 số đặc điểm cơ bản trong kế thừa, các bạn đọc và nhớ những nội dung trên nhé. Có thắc mắc các bạn bình luận bên dưới để tụi mình giải đáp. Pie~
