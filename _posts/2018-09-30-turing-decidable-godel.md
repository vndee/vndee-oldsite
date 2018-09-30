---
title: 'Bài toán dừng của Turing, tính khả quyết và định lí không toàn vẹn của Godel'
tags: computing-theory
author: Duy V. Huynh
published: true
---

## Tóm tắt
Alan Turing đã đặt dấu hỏi đầu tiên của mình vào mô hình siêu toán học của David Hilbert. Cuối cùng, người ban đầu ủng hộ chương trình Hilbert lại chính là người kết liểu chính chương trình này bằng những chứng minh của mình. Người đó không ai khác ngoài nhà toán học tài năng Kurt Godel.
## Bài toán dừng của Turing
<p align="center"> 
<img src="https://upload.wikimedia.org/wikipedia/commons/a/a1/Alan_Turing_Aged_16.jpg" alt="Alan Turing - Wikipedia" height="500" width="500">
</p>

Alan Turing là một nhà khoa học lỗi lạc của thế kỉ 20. Tên của ông thường gắn với giai thoại về việc giải mã cổ máy Enigma<sup>[1]</sup>, đóng góp quan trọng vào thắng lợi của quân Anh trước phát xít Đức vào thế chiến thứ 2. Chưa nói đến mặt chính trị lịch sử, những nghiên cứu khoa học rất quan trọng của ông đã đặt nền tảng cho khoa học máy tính hiện đại. Và một trong những đóng góp đó chính là bài toán dừng (the halting problem).
Bài toán dừng của Alan Turing được phát biểu như sau:  
> Cho một cặp bất kì gồm chương trình $H$ và input $x$. Liệu chương trình $H$ có dừng lại hay chạy mãi mãi với dữ liệu $x$? Điều này tương đương với việc có tồn tại một giải thuật $P$ sao cho $P(H, x) = true$ nếu chương trình $H$ với đầu vào là $x$ dừng sau một số hữu hạn bước và ngược lại hay không.  

Vào năm 1936<sup>[2]</sup>, Turing đã chứng minh được rằng không tồn tại một giải thuật $P$ nào có khả năng tính toán bằng máy Turing<sup>[3]</sup> để quyết định xem một chương trình máy tính có dừng lại hay không. Máy Turing là một mô hình tính toán hình thức được đề xuất bởi Alan Turing cũng vào năm 1936 để mô phỏng lại quá trình tính toán cơ học của con người. Và đây cũng là mô hình chung của máy tính điện tử hiện đại. Trong một bài viết khác ta sẽ bài kỹ hơn về "vẻ đẹp" máy Turing. Ở đây, ta làm việc với mô hình máy Turing đầy đủ, tài nguyên tính toán là vô hạn, tức là không có ràng buộc về bộ nhớ cũng như thời gian chạy chương trình. Ta thử xem xét hai chương trình đơn giản như sau:
```c++
while True:
	continue
```  
```c++
print("Hello, world")
```  
Rõ ràng, chương trình đầu tiên sẽ rơi vào vòng lập vô hạn và chạy mãi mãi. Tuy nhiên, với chương trình thứ hai thì sẽ chỉ dừng lại sau một bước. Chương trình càng phức tạp càng khó phân tích, ta có thể cho chương trình chạy trong 1 ngày, 2 ngày, 5 năm, 10 năm hay 1000 năm nhưng chúng ta cũng không thể chắc chắn rằng chúng có chạy mãi mãi hay không, hay chỉ dừng lại ở năm thứ 1001. Đó là một ví dụ về sự bất khả thi giải thuật $P$ để kiểm chứng một chương trình bất kì có đảm bảo tính dừng hay không.  

## Tính khả quyết
<p align="center"> 
<img src="https://upload.wikimedia.org/wikipedia/commons/7/79/Hilbert.jpg" alt="David Hilbert - Wikipedia" height="500" width="500">  
</p>

Tính khả quyết là khả năng quyết định được của một bài toán. Một bài toán quyết định là một bài toán mà câu trả lời của nó là có (true) hoặc không (false). Một bài toán gọi là khả quyết nếu tồn tại một giải thuật sau một số hữu hạn bước có thể đưa ra câu trả lời của bài toán trên. Bài toán quyết định (Entscheidungsproblem<sup>[4]</sup> hay decision problem) được đưa ra nhà toán học nổi tiếng David Hilbert<sup>[5]</sup> vào năm 1928. Bài toán của David Hilbert có thể được phát biểu một cách dễ hiểu là "có tồn tại một giải thuật nào quyết định xem một phát biểu toán học là đúng hay sai trên một tập các tiên đề cho trước hay không?". Tức là có tồn tại cách nào để kiểm tra tính khả quyết của bài toán? Trong sự nghiệp của mình, Hilbert đã dành nhiều thời gian cho một chương trình mà chúng ta hay gọi là "chương trình Hilbert"<sup>[6]</sup>. Trong chương trình này, Hilbert nuôi tham vọng thống nhất toàn bộ toán học bằng một thứ gọi là "siêu toán học (meta-mathmatics)" gồm các hệ tiên đề thống nhất và phi mâu thuẫn. Ở đó, mọi bài toán đều khả quyết. Ông quyết tâm đến nỗi khi chết ông yêu cầu được ghi vài dòng lên bia mộ của mình rằng: "We must know and we will know". Thể hiện ý chí tuyệt đối tin vào chủ nghĩa hình thức sẽ tìm thấy vị thế lớn lao của nó.  
Tuy nhiên, bằng bài toán dừng của mình, Alan Turing đã chứng minh sự tồn tại của những bài toán không thể quyết định trong toán học. Gián tiếp làm lung lay siêu toán học của Hilbert.  

## Định lí không toàn vẹn của Godel
<p align="center"> 
<img src="https://upload.wikimedia.org/wikipedia/en/4/42/Kurt_g%C3%B6del.jpg" alt="Kurt Godel - Wikipedia" height="500" width="500">
</p>

Nhắc đến Kurt Godel, ta không thể không nhắc đến 3 định lí về sự toàn vẹn của ông. Đầu tiên là định lí toàn vẹn<sup>[7]</sup> được Godel chứng minh năm 1929, lúc này ông tham gia chương trình Hilbert, nên ở một khía cạnh nào đó định lí này ủng hộ "siêu toán học". Tuy nhiên, chỉ trong 3 năm sau đó, năm 1931, Godel phủ định tất cả và cho ra đời 2 định lí không toàn vẹn<sup>[8]</sup> phá tan tham vọng lớn lao của Hilbert, lúc này chàng trai Godel của chúng ta mới 25 tuổi.  
Định lí không toàn vẹn thứ nhất của Godel phát biểu nôm na rằng: 
> Bất kì một hệ tiên đề nào dành cho các hệ số học như số tự nhiên và các phép toán cộng, trừ, nhân, chia đều có thể tồn tại một mệnh đề mà ta không thể chứng minh hay bác bỏ.  

Điều này trực tiếp làm sụp đổ khát khao tìm ra một hệ tiên đề nhất quán và phi mâu thuẫn dành cho số học. Godel chứng minh bằng một mệnh đề rất đơn giản mà ai cũng hiểu được, đó là nghịch lí kẻ nói dối (the liar paradox)<sup>[9]</sup>. Mệnh đề này được phát biểu bởi một anh chàng bất kì như sau:
> Tôi là kẻ nối dối.  

Nếu mệnh đề này đúng, thì rõ ràng người này đang nói dối. Mà nếu người này nối dối thì rõ ràng mệnh đề này sai. Vậy rốt cuộc mệnh đề này là đúng hay sai, điều này là không thể quyết định được. Bằng logic trên, chúng ta hoàn toàn có thể đưa ra hàng tá mệnh đề khác mà không thể quyết định được. Ví dụ như:
> Anh thợ cắt tóc trong một làng nọ quyết định sẽ cắt tóc cho những ai không tự cắt tóc cho mình.  

Vậy câu hỏi đặt ra là liệu anh ta có tự cắt tóc cho mình hay không? Chỉ bằng những suy luận trên, Godel đã khiến cho chương trình Hilbert thất bại ngay trong chính số học chứ chưa nói gì đến toàn bộ Toán học. Như vậy, toán học là vô hạn chứ tuyệt nhiên không thể mô tả bằng những tiên đề hữu hạn.  
Định lí không toàn vẹn thứ hai của Godel chứng nói rằng không thể chứng minh số học có tính nhất quán. Hệ quả là các hệ tự quy chiếu sẽ luôn tồn tại mâu thuẫn. Bởi vì chính nó không thể giải thích nó. Cũng giống như việc hình học tồn tại dựa vào trụ cột chính là 5 tiên đề của Euclide. Nhưng bản thân 5 tiên đề này không thể được chứng minh hay giải thích chỉ bởi hình học đơn thuần mà phải dùng những hệ tiên đề khác bên ngoài hình học Euclide. Đó cũng là ý tưởng của hình học phi Euclide.

## Bình luận
Bài toán dừng của Alan Turing là rất quan trọng bởi vì đó là một trong những bài toán đầu tiên được chứng minh là không thể quyết định được. Đó có thể xem là một lời cảnh tỉnh đầu tiên cho siêu toán học của David Hilbert. Nếu không có bài toán này, rất có thể bây giờ chúng ta vẫn còn đang loay hoay để cố tìm một giải thuật thần thánh nào đó như theo lời Hilbert rằng có thể quyết định một input bất kì là đúng hay sai. Nếu Turing cho ta một bước khởi đầu về một tư duy toán học khác đi đa số còn lại thời bấy giờ thì Kurt Godel giúp ta chứng minh rằng có vô số những bài toán như bài toán dừng của Alan Turing. Tuy nhiên, bên cạnh tập những bài toán bất khả quyết thì vẫn còn nhiều bài toán mà ta vẫn chưa biết là nó có quyết định được hay không. Đó cũng là một lớp bài tóan khá thú vị. Ví dụ như Giả thuyết Goldbach, giả thuyết này cho rằng:
> Bất kì số chẳn lớn 2 nào cũng đều có thể phân tích thành tổng của 2 số nguyên tố.  

Điều này đã được chứng là đúng với $400,000,000,000,000$ số đầu tiên bằng sức mạnh của máy tính hiện đại. Tuy nhiên không ai biết chắc được rằng liệu với số chẳn lớn hơn $400,000,000,000,000$ thì nó có còn đúng nữa hay không. Hay ta có thể lấy một ví dụ đã từng được coi là không thể quyết định được là định lí cuối cùng của Fermat<sup>[10]</sup>. Định lí này được người ta tìm thấy trong một bìa sách của Fermat với lời nhắn gửi rằng ông đã chứng minh được nhưng không đủ chỗ để viết ra. Điều này đã làm đau đầu và thách thức rất nhiều nhà toán học, nó được xem là một bài toán không thể quyết định được cho đến năm 1995<sup>[11]</sup> khi Andrew Wiles thành công trong việc sửa chửa những sai lầm của mình trong lần đầu cố gắng chứng minh định lí vào năm 1993.  
Mặt khác, những phát hiện của Godel và Turing cho thấy máy Turing vẫn còn nhiều lỗ hổng chưa thể giải quyết. Mà máy tính hiện đại là dựa trên mô hình của máy Turing phát triển lên, cho nên chúng không thể hoàn hảo. Rất có thể trong tương lai những mô hình máy tính khác như quantum computing (máy tính lượng tử)<sup>[2]</sup> hay bio-computing (máy tính sinh học) sẽ giải được những bài toán bất khả quyết và mở ra một chân trời mới cho khoa học máy tính nói riêng và toán học nói chung.  
Tuy chương trình Hilbert đã thất bại, nhưng những bài học và giá trị nó mang lại vẫn không khiến ta thôi nể phục cũng như tôn trọng sự nghiệp của David Hilbert. Có lẽ ta sẽ cần một bài viết khác để bàn về nhà toán học này.

## Tư liệu tham khảo
- [1] https://en.wikipedia.org/wiki/Enigma_machine  
- [2] https://en.wikipedia.org/wiki/Halting_problem  
- [3] https://plato.stanford.edu/entries/turing-machine  
- [4] https://en.wikipedia.org/wiki/Entscheidungsproblem  
- [5] https://en.wikipedia.org/wiki/Hilbert%27s_problems  
- [6] https://en.wikipedia.org/wiki/Hilbert%27s_program  
- [7] https://en.wikipedia.org/wiki/G%C3%B6del's_completeness_theorem  
- [8] https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems  
- [9] https://en.wikipedia.org/wiki/Liar_paradox  
- [10] http://mathworld.wolfram.com/FermatsLastTheorem.html  
- [11] https://nguyenhoalu.wordpress.com/2014/02/12/dinh-ly-cuoi-cung-cua-fermat-ve-nhung-dieu-ky-dieu  
- [12] https://vnexpress.net/tin-tuc/khoa-hoc/computer-luong-tu-co-the-xu-ly-bai-toan-khong-giai-duoc-1985779.html
