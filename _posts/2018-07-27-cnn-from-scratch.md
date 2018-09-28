---
published: true
title: Convolutional Neural Network from scratch
date: '2018-07-27 02:57:18 +0700'
comments: true
categories: 
    -   "deeplearning"
tags: deep-learning
author: Duy V. Huynh
lang: VI
description: A tutorial about Convolutional Neural Network with Keras and Fashion-MNIST dataset.
mathjax: true
---

## Convolutional Neural Network
![Intro](https://lh3.googleusercontent.com/lPExPbUljLjBXEIQd54kyVFZ4Z36sFAXSPfI9H-RMX81qn83Vl-R7-fMT60p2PhVa0RjnEJBLwaWuqvL4TT0i8Og_QvhR3RbK6xxOIxrfijD2FXc5Nfzo8QODOu0hWlRZUk8tq7xxGfOasHsLdOfCXtTAu1D_ifAZPXF_-Ds-k1FOk9GlR5Dkkwan57wzM7Nvet1O084FpTwg49ihxEUM-gwuTwizKgrWSgfWiMpS4YvuBHenSFglbqZj66ENde_xhsKPjeb2Y49FYxeJGFcLNumOje692tSqInQLbQapVnVv6SLlER-up42jUIjYvdypmIoxZfMauTyC3RwYeohiAklISIQW0wrDAg2A552P8koG_NBpVJwgqiBRHtCXRwS2KxaHZvrXWBzPhyz_ulRTeyxOVpbuYkw7EKn3t9Gc4g0RC4-9CUbnaf49Wc3dWd6bp9ifcBXr3jQFEwQ66ACyFgVzeo45RgZje64t6B8v_lBcF1o3DFfgqzRBGhyKoS60Ji1yRnAv23Ln2ptXSB_vHSOHnaJtALY3Nb3vUPuSpBTlWL_ivb369wfz9TULcJ9bwSdoplGii5YrGN0Kv1o9xrS7uJHFrzGm0BxBu7NpatXV3OjRQNUJcEAA8hYY4f-H6xnlWS7OSO8iHo8jY1OakjASPd2HZ_R=w748-h279-no)
Neural Network ra đời là một bước đột phá lớn của giới khoa học máy tính và mở ra một tìm năng cho các bài toán nhận dạng. Tuy nhiên với những bài toán có dữ liệu đầu vào là hình ảnh thì việc sử dụng mô hình mạng Neural Network truyền thống còn gặp phải nhiều hạn chế. Do đó CNN (Convolutional Neural Network) đã ra đời và trở thành một trong những phương pháp chính để giải quyết các bài toán nhận dạng và phân loại không những trong lĩnh vực thị giác máy tính (Computer Vision) mà còn ở các bài toán về xử lí ngôn ngữ tự nhiên (Natural Language Processing).

## Kiến trúc LeNet (1990s)

LeNet là một trong những kiến trúc CNN đầu tiên được đề xuất bởi [Yan LeCun](http://yann.lecun.com/) sau một loạt các thành công về nghiên cứu bắt đầu từ thập niên 90 của thế kỉ trước.
![](https://lh3.googleusercontent.com/VXTODFaGAK3HP4uX5WRzxOkzCSKZkDX9XQB6w7ReQzzD1SwPpSjp39tdvHxCZdT4M3pPM-17vDzLwfxfxsISiqOfnns-kltgnHy_cF5fCEPinbmJ-GEUiUAhaMT0Pxie4Da6Mm4yt-u48FNkYCkhqDx5V1EwJso9CCX6XEfZfXf7jLmzx1MPu76Z4Kd90clwsEHRTDNyCk6Iesbe2Xb5u96gw4GOQz-H2GcNb3SLP8tCTIIx6LESIPfuyYB1GTxug5bJlLtV_6_Pm_8L-N-F1mPbbUGhL4nKAWOGtjQrxDlRha4L0i_6kPjpGcsg_xAD6AUFqTDT7Qr_B5FjYsCIK9bNiGx_StQf06SLfBsh7N29UugWZ2zeXvglGjmCoWzEyua6VeikipznQTSAqntDQs14oFlSYtLUu1jA98OAPl68s3VULBinkLUvQoZjQMXvSvD3kVlRXg4RCWc5azNTp_jPeLjIUNXv7HrsjQmchZKU5ttGwU0dHhM_3ElvzOWMkaVRY3bPB_QP3dnues36_FjHtiB2G5OsyRgOi_5rA8XA0vrCVax3M0QUHz86JgARtk4ztPNN7PwV63QHr2gNj8Y_S8hbOEKGHLMMNSm9OCpnKSNIb_Yyn5dZRRg7X4o3HmGe6kp-vV0TiNjmFuSydU7baDtSLO5S=w748-h178-no)

Phía trên là hình ảnh một kiến trúc CNN cơ bản dùng để nhận dạng vật thể trong ảnh. Mạng này lấy ý tưởng từ LeNet kết hợp với một vài cải tiến trong vài năm trở lại đây. Dễ thấy kết quả đầu ra (output) là khá tốt, khi xác suất nhận dạng được có một chiếc tàu (boat) trong ảnh là 0.94. Trong khi với 3 sự lựa chọn còn lại (dog, cat, bird) thì có xác suất khá nhỏ. Điều này đã mô tả đúng ảnh đầu vào (input).

Từ quan sát trên, ta định nghĩa một mạng CNN "kiểu mẫu" sẽ gồm có 4 thành phần:
- Convolution
- Non linearity (ReLU) 
- Pooling (Sub Sampling hay Down Sampling)
- Classification (Fully Connected Layer)

Bây giờ chúng ta sẽ xem chi tiết cách mà CNN hoạt động.

## Convolution

Convolution hay tích chập chính là tư tưởng chính tạo nên sự khác biệt của CNN so với mạng Neural Network truyền thống. Vậy, tích chập là gì? Theo [Wikipedia](https://en.wikipedia.org/wiki/Convolution) tích chập của hàm $f$ và $g$ là một phép biến đổi tích phân đặc biệt, được viết là:

$$ (f*g)(t) = \int_{-\infty}^{\infty} f(r)g(t-r)dr $$

Dưới góc nhìn thống kê, thì tích chập chẳng qua là kì vọng (Expectation) của của biến ngẫu nhiên với hàm phân phối xác suất cho trước. Để dễ hiểu ta hãy lấy một ví dụ: Giả sử ta cần xác định vị trí của một thiết bị có định vị GPS (để đơn giản ta chỉ xác định kinh độ). Thực tế GPS không bao giờ hoạt động chính xác 100%, nghĩa là khi đứng yên thì tọa độ vẫn dao động. Do đó, ta cần một phương thức để ước lượng vị trí hiện tại từ thông tin của các tọa độ ghi nhận được. Ta định nghĩa một hàm $f(x)$ là tọa độ của thiết bị GPS ghi nhận được và hàm $g(t - x)$ mô tả trọng số của $f(x)$ tại thời điểm $t$, $g(t - x)$ càng lớn thì độ tin cậy của $f(x)$ tại $t$ càng lớn. Rõ ràng kết quả mong muốn của chúng ta sẽ là:

$$E = \sum_{\forall x} f(x)g(t - x)$$

Nhưng nếu $f(x)$ là một hàm liên tục, ta sẽ có:

$$E = \int_{-\infty}^{\infty} f(x)g(t-x)dx$$

Cho nên có thể thấy tích chập cho ta thấy được sự tương quan giữa hai hàm số hay nói cách khác khi hai hàm số càng giống nhau thì giá trị tích chập càng lớn.\\

<p align="center"> 
<img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Convolution_of_spiky_function_with_box2.gif">
</p>

Trở lại với convolution trong CNN, với dữ liệu đầu vào là hình ảnh, không giống như những gì chúng ta thấy khi nhìn vào một bức ảnh. Máy tính sẽ nhìn một bức ảnh như một ma trận các con số có giá trị từ 0 đến 255.

<p align="center"> 
<img src="https://lh3.googleusercontent.com/CHzhgT6LqkFnIUgvBp4xf9p8j3joMV4K5LUg1Fw0RScaWnY_62fn6s6vt-U7lsUIw83YT958ija01-KI3oQsPjcnYhObpcrB7FAD9DGrBDFS-D8qk5C8B9JycXGuC5i6OsZPan_kmKV2_rUU2BNUZl0AkegPBtoNUjf-qPUd-b8AaBJTvuamJ9QUJ3gh8s2UEfVX881wsGtjsoCnICvGQR_xMYKFb4DZq0R7frS2Rz47oW_okII3IlgH0OM6KbO7egvvLPvNNKVWHpYiTwxGcRlcGjDso1rBtzkIsXWhmpn0l_-77GWa_EkPVeWb8355-em_p9VGN1Dbx2oGpEVY-kB3ktZIQAjIlIPbSyVytYQkjCfUak-P6VTU8Om9L1LdIPSebn6vxVHfoRK3zypTvmkjdGp4uSvgSWFq3a-p_6Wp9pDbDxhC-Lm0l4FXbDHBi3mNqmNMdcvjVrpL0kEj_MshEoGD2gxW8W0OZEjGByvO1xOkFG6jY5wQ6mP6H6yk6i545N40Z3K3AGPZUCHk1u8X4DJkvrlZ-h5F5UQFQqO7fYrVAOCfmTFgABt9L7Cjzqj9o9XduMQAwN3b1Pt2DRq5fSX2xrGBfIEIUGA-UFnlkskiwdT76VjMApTmFyYaUxpat4dykyx-qNttqrnL9SiVSbaSlslN=s192-no">
</p>

Channel là kênh màu của một bức ảnh, một bức ảnh tiêu chuẩn RGB có ba kênh mà đó là đỏ (red), lục (green) và lam (blue). Cách biểu diễn một bức ảnh RGB trên máy tính là một ma trận $H\times W\times D$ thay vì $H\times W$ như một bức ảnh grayscale (bức ảnh chỉ có 2 màu là đen và trắng).

<p align="center">
  <img
       src="https://lh3.googleusercontent.com/_cfxkDvDsi3u7Fk1SuAu8R9og07kLn_LCggShg9HcHv-vxLwUFIFxi9BQ6km0vqdLXNalcRofE0Uay3GkI3wiAWC8aVCgo0x-qf9ZM_hgP3SNC1T58wC7xIV3dTql7lO3XxiZIJ8R2psxCvT_pAfFr3UpksSHaIY7kGgehCJnyq8ow5kBlFavPS7ES0yBrV-T97SupBRiPbBNTlcC4rAzfMCETRG1f7v27Sv51FKFLdVTbp7dzU7wn4UXlbkZDr2Ho9zuXQJ-4fYtv02ljFShKDDOJQXwh6PLjC3d90rwobLbrccGG8KDJuXJVEe-ZSGTsPV4LnWGz6nH2Lw9pk5iPDWiom02ERZUfNeH9CMbaaUz_WEWu_Sal5DDHvZDhTAT0HkvMLWLc6NXbWiO9EBsozT1G901zbnctF3m39-oARupTIdoeEtDCNy4tSo3-e_pu13VLHzgrn03W2yPZVXgp48Ib1vnmFDUMHkRqsmvGtL3vVSLKZb6ERv2qqqAOA9vAtppOhRWZ9cjysbVEqysyfK2G9i3Fle_3G_kL-yZjh118u_5MBKALQrjyfzlgCNQNYjqT7mwmJSB-44HE-ZIuVXKAc567QdiifTSuji0QchI6vUyMP9T8ZBKlAoBaM_eAUQXZ4Eij8QgbZKMqN1bWK-UpxloBr3=w231-h244-no"
       >
  </p>
  
Ở Convolutional layer trong kiến trúc CNN, mục tiêu chính của nó là phát hiện đặc trưng từ một bức ảnh cho trước. Đặc trưng ở đây thể hiện mối quan hệ về không gian giữa các pixel trong một tấm ảnh, đó có thể là một đừng thẳng, đường cong, hay một đoạn gắp khúc. Để làm được điều đó ta định nghĩa một filter hay còn gọi là kernel có tác dụng như một sliding window trượt qua toàn bộ bức ảnh và tích chập với từng vùng nó đi qua.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/eZTgfmBw6CgL_n7Zde39IXm4F9msW-gVjvAzPViRrIecG1rROvxXCyDnW3ZFHtmAr3ImfGnef0I3OUDLfZA0HMfeXpqVcPBDjVED4h7wqfvFjZwK-YC0rw7_lchu3FQU4ibXd6HIz5h502hM2Sif2jv6rQNlNiXPKDvqSF7wkraO4jfc-dp2NV_7RbJkn967b0PPzo0L-0EQiyogiGA58O-Kpy8kNxoM1lQ_RJL2TCCfogy-kAGNHW2zNojcb9BuQH5zwJEdd3uQAQ3vadoCmTsoAuCNg3321aUs_1_lcoMJM0NecSDGodp3IyjirixDXue9fk1AqwVY3nTuYxpJFfiM8o11-5bOZy7mJjR-xTxj2auc5kGYrncw7U6NmpFyz-nGzFaMJjivmS5dT8Dz8-hfZnJ86RQTmzMihk4v1wqILMXJ84G-Yn3oV_5ZoIDqFTSB6BX29a-xO1BFT4TAvly5kLzzos2wPl9QnjraGg46EVtbDZIxtZr_GrfErWeLMBhrVhk6Drb4H65Il3GI46FF_qFzytBwFUTiPYVxgC8KOVxREnNCtsRK2biWF0aP5pW2OUgA1NA17Wn783VoVmYkNKvPwtUc5ncP1VCx5B8vYuwBagjuDoHrWKnLMuHYRh9b72Kt8fawVaJZUxRWV8A2yD8OA41-=w461-h243-no"
       >
  </p>
  
Chúng ta cùng xét một ví dụ: 

Cho ma trận $5 \times 5$ biểu diễn một ảnh grayscale:

<p align = "center">
  <img
       src = "https://ujwlkarn.files.wordpress.com/2016/07/screen-shot-2016-07-24-at-11-25-13-pm.png?w=127&h=115"
       >
  </p>


Và một filter kích thước $3 \times 3$ như bên dưới:

<p align = "center">
  <img
       src = "https://ujwlkarn.files.wordpress.com/2016/07/screen-shot-2016-07-24-at-11-25-24-pm.png?w=74&h=64" 
       >
  </p>
  
Sau đó cho filter trượt trên ảnh và tích chập:

<p align = "center">
  <img
       src = "https://ujwlkarn.files.wordpress.com/2016/07/convolution_schematic.gif?w=268&h=196"
       >
  </p>
  
Kết quả nhận được là một ma trận $3 \times 3$ ở đầu ra. Ma trận này được gọi là Convolved Feature hay Feature Map. Phép tính tích chập trong trường hợp này là tổng element wise giữa hai ma trận cùng kích thước (tổng của tích các ô tương ứng). Vậy câu hỏi đặc ra là làm sao để filter có thế phát hiện được đặt trưng của một tấm ảnh (Feature detection)? Để giải thích điều này ta cũng hay hình dung filter là một tấm ảnh mô tả đặc trưng, ví dụ dưới đây là một filter mô tả đường cong:

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/0cTydRFzJkX7oqrytNbw9ERP3xyMzMkbeEt5JbRErX60ERD8-X_BAIUILaroyIp_GNFxa_0N_7FCIvz-XgdyVpZDpniWdCvuEn1Hxj92EMCCF5eCyhO95rBsKa0TGmTxDrGIIjPz-6J7pw8Mys3uxbo847DaMcXPw_ouwuVSv8_6QLp7mabeuK_bztGyFDHrTBmECV4S5W-o971AbKlvNrnuqYWjkzIJDuz3d1rx-IQjgDr_yH_M9f_GIZbMf2xP5i-zRNGzVuojyMctEcD5YzFevXqHdAuEkQfjBRUc_xGBaWUKSBAbv5Ir8vfK5EK15VoIOo1_5FjJUOeLwsHaimgwjMt36JXtgWaX98XNiSuqFrJLTzpK45zBnJy8ah4UPdJjrGW0u84ikRfmIo4fEAaLJjt23g3yGOUXGJD1vEBM7bDFXu5-5Mo08icpvxMpr57_h7XBfdNbkxf6N2_CxJL4NxCjxWDcHiyDbPj8QERqf6BqKnvWuE4OFiik5oSLI4lbGjtFUEQdysX-WDZDR6kbgw294ykEh8LCf05uOvs3KbYE4Arb0FIxnaUF9Fiw7rNQpHvUEcKQ0na89KOohBvUUzlJaVKqL7KygL_b6Dj9wokakHOKQqEoPrg7W7UiDKGFdbl-QnYfMlaerEteBhFt84V4AQlu=w624-h278-no"
       >
  </p>
  
Ta cho filter này tích chập với phần góc trái, trên (ô vuông màu vàng) của ảnh gốc.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/ee65AFu7qqPJTr7ie0Z2-IQFSIawdEDFXGiVESsjCBsItIqdARbIKNig8Mhjx7nPVgZ2UsE5xaY60PpTzVs9Qywd3Ale32lovJqyXlaOZz0mfpaOO8kESlpB-IJMm0IgURugoc7VUpF7wFxhlYTf4OUyrTU-29_0AY6srrma93em2i2JaYzJ4aAhWOUfubKk81zPGWHamZJsoOmXqGVyi7MW3IyIe45qen82PRHpHdNh_lBbTWWzwpJ0uUzH1fUIEAYdXgZZ2q3_68wkBgZjdyCLx5JaOjLlTcVtMk5DF29_o8orZmgUVydMIsWk9gJJUmsln1us1zDPTjHkS_yXx0l3sxX3T_alniAI93jlmcxzVmbl9AHEZTqS4OGhg0ucgsqTLC9jaDMP_h6XGjTvBvuYdGgE3GAdzDnrHLZO3Ew22itqw1o_T2vJJ4rC76u6tFpagSZIcGYrl8DmiygFrbHlfux2g-FSsqFyJEnvqCkrLcGjqxypnqx_AHaTdH7AttlBAenNcFDabxQvURmTpskwa_0O1ZoYe-BXl59vbJe_ZeULBwBw1j_h-8nqLMMJxWukbO1lXlBc1ARYqx3OogH-2e481BCdH-WlS-35Rtw-HsgWbTDl5AqMrd0PjNxL32c4lf6yEBGDo8cAbQUb8c0XH2fLk9ga=w645-h229-no"
       >
  </p>
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/Y7Xoeoc67R4wbPAiMTNukbEztTt-Oitay8eYGehVNO-jvEj31mAl1F0HQSheDsuXckHmTQ7CFl4QpLhyj32ZEQvKkf2xVSC1n6uXtPteoJwo1EwgbCn-nQRB9HCpTj2Y54DH1Cv0aiK0ix9B1K1_1jONXaJSeEalUzgWgZ5LGqZh76-_CIsmt3Y-IN1Q19Fe-cpK4JK13Tq5_FUDPxy7xzCY1eS5LC7_VYKRrkTmQPbEue5g_ueRWC-gRa_cPPpYq-Mq0XPZamwSkRNSPFdnEKrokNj3r4aOSwhlwopEC66927NtS1UFXyVPR0JKmYzriGiaVLO1zojvfPAe2Pg8852B3GpOcblNTqSvwOjes59Grq37BgcUjnC3Zimu1nlZ7g5CDtyDvOso_1zUsnshX3xHH6B3P2n3EZQFBDHGZuGCaj0xQOv3eNLIMJtPNuj7BGZ-EV99WGPKZfFqWRbMb_zB9W97caH8X4NYe40vTMmlELMmNGWJElMlgZnFqYDgOvxJnslg2vYoOQzmaXfqxIsOIQzSQAz5p94JKLGlzr-isy5pjU0C7HdIL-OleigyrCVPmfd6KMcy99kTi46jkpqsMgYUfGaLsSQGAqfeet8d6aVJl7fnLg4kB6a8K8c_hdlc-D8eMpVwOleUt0B9f9l-CuKu8tOp=w711-h268-no"
       >
  </p>
  
Kết quả phép tính tích chập là $6600$. Bây giờ ta sẽ thử filter lên một vùng khác của tấm ảnh gốc:

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/fXxIlkanhcV2yrdG4_73O9q8T5tbM8eiApRtS2VpR333rcLqykVrO7Hg2NaOBNO5sw5_DWDJhbZIaFFLlRAunDitFoVWsYC6UZ2gbn5Pa1TdSPU1Aw7OS1DI6Csrjr91TcX882inSXwb0WL5tspjCkVqFa4HqlX8MgzbSzgxumhQZjv1ezQSmEKlbF0KxlrSlwAkpaL3rmEam_Q3_PN8e4T20Z0fpJ4ZAGO-OT0qIWu8ceem0sLyJoTQFFcNae7ySQHQ2366eUu9YnzX1IZAB5mi4P2WLz3p8jgts9PgisRXP1vWYtKQ9sKQRm6mmDoZ5V-O3_6f9pDE-EN9JryP-LIszgt3Y20Wdtse02n9UQ36SmLOE59Bq-qMs7HmHaI5yrIM8hlxYk2xMDWtqTm1LmmGKv-8LqoUYEI_EBqO3XwgPpnAK06nPNcZ99qBZryj6iKo07-5aN1KUcj7Zb6gd-uPv7d6vAlHB33AlmYJTnFMh8zRpXGSXmQIya0momC1soT_8LVIKgDCeYtqWnwh-4CMoh4bQWkDxwhdFdwOttTHwqTcv7HFzmixbFHydUhh1e8CkYPqCf5gNLqRSoiYbOlGmHisngq0DqZJGSS8pTYN4KdV97SQeZn2FreH6KcVlSr5uDGFeIG1gsBALLTrJhCPE7c3Npuy=w718-h251-no"
       >
  </p>
  
Kết quả ở vùng này là $0$. Do đó ta kết luận rằng vùng góc trái, trên của tấm ảnh khớp hơn với filter hay nói cách khác là xác suất có một đường cong như trong filter ở vùng này là rất cao. Tương tự như vậy, ta có thể dùng nhiều filter khác để phát hiện nhiều đặc trưng khác của tấm ảnh để có một cái nhìn tổng quát về nội dung bức ảnh. Ta có một vài ví dụ về các filter phổ biến hiên nay:

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/z-6rHekkm-BrwvkPof9B80-4Gt-UM4v5L0mBIhQYOKDf1EaRBZLmaWTf29Do7UBHhnPjOjTWLUy3fbVXW3e1ky8c_lMe--vgkq291GEQgFQEH111eSdkJwWMSSGPc2r3Y3O7jZesoC-Np04QdTzCDshXLPShnCk5DFftk7DsA_Ts33l3F9nt5LjXqHm9gmerY5wIkOq2zO8gZU_SmwH__7cLqWUeT00DRCERE53QfsqXvBNQvpNaUjRqmPiqG4E5nQUd0i_Zm9pB8HOe-SvzQ1m3C6pgH8tty01AAMqMwpt94BzSMl0f1LHqbjI5WMPsGko_tOrhShpOfavTXiB_LbsH_JugA87uxQxsEkfyplcRkg36-S4SOfq7jH6IRIhG8-v5W7fJ8dq3cs0K1jDgT8EbjE353XN93W2k_rVg3NqBn4GOOMOU-sK3NitoGiqyLnzh4hOc68GkcBIiOdJYNMLmowv9Ftk-MCikfO_LwwNKUATXWZdqMEICqdmoL7F31zDUHUwEabv3drZ9FW69kZ8qrGJrPX_4r2ftYupRWL-WcHuHTRVDA2rJSFfbmNgv4vnwq3U-yTGTTzNNijL9NZ9jX1gapiiscnKcUdjsupXCwXyO7RWUL2HC3fK-nrb-oJknb2Ru6g0ndtzS1MrQXtm5gA4UqXiy=w67-h66-no"
       >
  </p>
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/uKPMzKui-9AT1563MMmbGqSTWEbL86OMMiX7Kq5WQ7uTNUTpOme3TykzBYYGex3DJavqaF3OkJ22x1EzFu6d3aTV5pUYqLBguU6Zi3QFLSvfVD1A925G0_HVIlgQb1Vn-jhg62uePbrDVHqegleb5M5PPNB-XYFBB-q-WfI2qoRgtcjfaullXu2OXh6XkLhguVO_WZxltc4lVA8ITAhvIYZOo4n29PHvLXWk0m5WVDJBJKA0sBZ6yw9Z8O4CxHllB3JA6wyczkHLKBaQwL7W7wT0xDzmDt3PRsXY5gUeXma-QyuNWDTZTmBwJXYwtQ240wERO2Fp8FjZksUxubw5v2EgNFntoo_lJ2rTfaoqeqD8uCQFUOlMPI5dNGkPjGsHdoc0qtJ7M5036raiQRgO-Of5RSIlisSRutgQNWzmssCBHb_JgBYImeNpD0IUFF0D9NnQ3RElsk6rCfFGNb5eJRrVtfNVdfDOqXI740sZY-iV9wNtRvCuieGPPTaw_mHqXtPah9Rhr25GoHdFujwiR_goa2PResLv4QBIedkX59ywSCrGVWdVezEJOrFRxv_K7SEIDhxbfon52vY8e8d3XW_KK8wCqzV0T28hHuWT9AA7zruWqGZA37OZZutU65WUlzw4XL0t1034XbAFqYqkPWG6el84OaJ_=w342-h562-no"
       >
	</p>
 <p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/Yv1UcvPApQZSZJH01jkLduxFgKwCFEFyzUiRziP651QWPyk6HUt99jg2htGqaLBHXuqKLtpB10MWQhXdcpF_ncYAj3RlOtkzqr_k6JCTZ7yxHCO4IP0K88dNoSv1GAgWviuT2QuMOQ98Kyewa7uvCsjHykRtpjFQFEQRlqr20J8bcIZAgS9PkMfeQXtrXcLorVUpujQGbxjzrm1hX55wlY4T3m2utiuL2k-07l1AdQGGmiDQXqllFGPOWDvUZx_GDzzR9sKF6AG33ViMS7DUVaHShQizHvqMcIrOaRP31P5sucIKHfgdqMswYF6Hmc7_DvLUlneQt1dsjiPEHMXsoGgOwe0R8MmFfV6chICeqWOygEdJJ8oJdaNsapNSh_xLUOC0jLF_Yc1eCCwBDaEBpRUPTXWy9StPe0xwEV8uPqOJxShh31lhnwdfzFft-_AdDq6Wm26uUNLhVEdVYJvsiYQtAvaxZzM8h08duqOo8SSYD6jufopZRI7yhZRafETBS5jNym0xeDWGeqZrKtJcDwge139PTSxR4k3jRaIMl5TrqRTHr3p4rpx32Eiq5gUdPD5j5btczpfrFJT9pRDrMQSIPTxpW0SNAB4R9vA2kPRORtiyTy5Uix6toAAnWsAG3s4LIdjq4gnH3By859xUmgjQRp6BI7wt=w480-h272-no"
       >
  </p>
  
Trong thực tế, filter chính là cái chúng ta cần learning. Learning các giá trị của filter sao cho phù hợp với bài toán là mục tiêu của quá trình training CNN. Tuy nhiên vẫn có một số tham số chúng ta cần gán một cách thủ công đó là số lượng filter, kích thước filter, số lượng layer,... Càng nhiều filter thì hình dung về bức ảnh sẽ càng chi tiết và xác suất chính xác sẽ càng cao.

## Kích thước của Feature Map

Feature Map hay Convolve Feature phụ thuộc vào 3 yếu tố sau:
- **Depth**: Độ sâu hay số lượng channel của bức ảnh là yếu tố đầu tiên. Độ sâu của Feature Map nhất thiết phải bằng với số channel. Vì thực chất một bức ảnh RGB sẽ được biểu diễn trên máy tính bằng cách ghép 3 ma trận pixel lại với nhau như đã đề cập ở trên. Vì vậy cho nên cần phải có 3 filter khác nhau cho từng channel.
- **Stride**: Lẽ dĩ nhiên kích thước bước nhảy ảnh hưởng trực tiếp đến kích thước của Feature Map, cụ thể là chiều dài và chiều rộng.
- **Padding**: Padding hay Zero-padding liên quan đến vấn đề weight-sharing trong CNN. Ta thấy khi cho filter trượt trên ảnh gốc thì những pixel nằm càng gần tâm của ảnh thì sẽ càng được filter chạy qua nhiều lần còn những pixel ở càng xa (gần biên) thì càng ít được chạy qua. Do đó sẽ xảy ra trường hợp một số thông tin ở giữa tấm ảnh trở nên được "coi trọng" hơn (vì được tính nhiều hơn) là những thông tin bên ngoài. Điều đó thường dẫn đến việc bỏ qua nhiều thông tin quan trọng, vì những thông tin quan trọng không hẳn là luôn luôn nằm ở giữa bức ảnh. Để xử lí điều này, người ta thêm viền ngoài của bức ảnh những thông tin giả là những pixel gồm toàn số $0$ để cho các pixel ngoài biên lùi sâu vào trong hơn và đóng góp bình đẳng trong quá trình tính toán.

Đến đây, ta xây dựng được một công thức để tính kích thướng của một Feature Map bất kì:

$$(O_{h}, O_{w}) = (\frac{I_{h} - F_{h} + 2P_{h}}{s} + 1, \frac{I_{w} - F_{w} + 2P_{w}}{s} + 1)$$

Trong đó:
- $O$ là kích thước Feature Map (high, width).
- $I$ là kích thước ảnh input (high, width).
- $F$ là kích thước filter (high, width).
- $P$ là kích thước padding (high, width).
- $S$ là kích thước của bước nhảy.

## Non linearity (ReLU)

Non linearity hay khử tuyến tính là một vấn đề cần thiết đối với CNN. Đây là một activation layer đi theo sau Convolution layer để lọc các giá trị trong feature map. Có rất nhiều hàm activation có tác dụng làm khử tuyến tính như là sigmoid hay tanh. Tuy nhiên, hàm ReLU (Rectified Linear Unit) được dùng phổ biến nhất đối với CNN vì nó giúp train model nhanh hơn những hàm còn lại và tránh được trường hợp ([Vanishing Gradient Problem](https://www.quora.com/What-is-the-vanishing-gradient-problem)). Hàm ReLU sẽ fix tất cả những giá trị âm ở Feature Map thành $0$ do nó có dạng $f(x) = max(0, x)$.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/cmuAAP3f2DUnJjQ1XfDjHAssi202FZoBbHoFAFb-aXqckzw8xCtreJ3G---6IssGJuXZAUQhyN_Jmrn7i6IMb6KdXnB2oGgLqPEU1UhGyIBvzerEPyNcG5c5gOga2nw3LfyfL6Xf4F05guMawjb0rBG-p2i3xytSczMh0MEo4F1PGE1aGldiRaj9z-vgrE4tI86ZykUVWl2dt3yDdE6ZBNH7uPHOlNULp-4gOAdpCrXNx40ZR9hTCmg97Iy4XPB7voxrH6N3VbOEnjOr-GWyGc2tWmgKUX30ujK67U0H4pmbNRv5nZfR6w2-WnyX7F99xoQDEWOYNljvJYg7j6UnsmuBhAlQO_DuI3raTqMPk6jHYFkzRN3peCbJDtnZCjwwTTwfNJNOSGLA-GHJwCvBpFfY570DEGkwzk17AO2bIOKz_vNyBwfomAPU-kyqjExncCAo4P01sEKiBARWL4YhMTiVy3BHGUcf5AZ84thjaXALz_o2S3TDHhtpNZbzXgf4ZXDr21ORjJghMG5m1wtk65BSj5jmyDBcARzdpW19BidXsFpQogV4dQKzF6sXYzN4E0fRmdZrzyIe-8C5gl6Ls2nHVYMQNekzwSJ64kehatRIeCShTZbrZMf2yxX3MvbtL8PmQLay92RzN6WFptT6eacsrWC1n1E4=w537-h168-no"
       >
  </p>
  
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/j4nV4lt91aErnwyM4YRbZPd0j8Dy1lIaVkSotfl84AONvj4Qiw9JN79G4bFELwOI1998LV81Ss7NQKQYUedQzFRJDsMs5EpM-pGpCRdmQ4GTmOINQy9uuxkgtSU3DXJJRTA59VKeP0__CuOrbJL8GtU8G6FtinT20swkf_fAP_YHwovDyOrEvCyOcxTQsfymR8EDDaPUUhA-FiwVSUZ-Nn4i6UUyQ4Pu8mO-DVTfogt7hwc6JVSoxImAZ2_nAQ0ZwwHYMn-xzCsc3TQzPCuen4xOL5ka5FRSndzq51IgGyEjLWnvSb8hffKQ18Ghv5tSB9MRa-CVhJApsFL43n43P2PuvJzAJ9zkhrcFRo8xEkVqWFAHr2Pn1Dn5V_X_j4RppQN-Mk98OW_eFfrjjayQXfWzIN3jAMXreZ033_QQG5KOQbRJWjruuTR50NbWtPQYsoW-xTV4ZtCbxdAw0t3P9seyWwnqK1yLEgmp7It1tukBp-a7pcN2C1GnS6ZONkIvAwmjnlDfohZEx_dHZNm6QIqjIS-ZAlbIiwxfRUR1R8EYEE7WNKRgPkAWD5qMq1b3e117SFkaA8RT7okEC-l7l_PCTevMP-DHXNg_GHiQ8DO9cXn8mFt_DaV5F95y4YjToYSzGcRLtPsFLf5rurRZYHvYbVpPWm2Y=w449-h212-no"
       >
  </p>
  
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/gc-QoUtqCQOfuFGI7srrw7IZOGhQPhXcpLwUg_OBBqMRe6d_9Ij8_b5BResXDKaTYfCQKMAKU35dhK_o05tKvKPyCu2qL3dQYrIuBNNuiTd8DRSAfX_BjG_V9EGzzdo5ylUMFr8O-5vWwBoMRkVbfbAs4BvQlyDce_juwa3CX16nnwBB1apyDiFlXZYB_conAl8w8LOUrpp5CwwljuVMONARQBbCTL2SxIUdbtE8zMJcbObm4L8pBNDIVy1Ya24-GB7dAddIKAwdP4kIGq0OTFtcGQFi-VgUB8vhHRunXRNZ_L25IJSqHHoBX1dl4TOU2ifZNto4MkagvzlCoa75rAFJ79xh5Hm5x-wHNGFrZ86fUduobMmFyfztqMfwFALRxGVauANAy7GGyHnyBo4n4Ycz-XWb6-i-IOstEqVJGYBS5ZyPTi_XWrEn3kJIVC7HM-lraeRu91lUG3AEsJoyUnb7X-ZKPX7CISvNglEIIhAFGr70xbAGyH4Hj9w8pIGZbFXyLIkBZP4rTCqFkM9MRORxECSyaLRT_jjt-yMBv6_0-pK5tjAuTWvUR8LpjITo_aRjs9ODekd65tVtnBsY9AQVqeCYH9dQNs6sxXGoxJWsm5oM0SAS_BuGB_RotrXEKSBBzoFDrGSuzisnLTA_u80NSFboeejJ=w748-h280-no"
       >
  </p>
  
## Pooling layer

Ngay sau ReLU layer sẽ là pooling layer hay còn gọi là subsamling hay downsampling layer. Mục tiêu của bước này là làm giảm kích thước của tấm ảnh và chỉ giữ lại những thông tin quan trọng giúp kiểm soát overfitting. Những cách cài đặt pooling layer phổ biến là max pooling, sum pooling và avg pooling.
Ví dụ, với max pooling layer dưới đây, ta chia Rectified Feature Map thành những ma trân $2 \times 2$ và chỉ giữa lại những giá trị lớn nhất trong đó:

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/d6ey6RbxaVS5VJlXrC54t8NzYkXUbSPu_rwJeacDGIAdA59PAoFBiTugBH5RV6X9oxTlL7IA6BushLlcaNngVPzhSCvXkZqT5qkGBzWd4VU4FO8Q7GZQ-yGXQZf2WMWNJr1H88Dq_8n5Mmf3eWB9yL_UdXGbYUUwh_fmGOA-AKahuUE-CqKd-57eaMnVtbbqMftf1l55CIOHFYKD1s6VVg2QOep9Pu6F4KksZnAGz6i2PTvKo3FThKu5p4KJIREXNevfPs0EYDT-hE2hGtrwdIpQjrAGZ2hsNvK3kX-envdycctL9bYuL2BojIT0_x77G-ChFCcA7gsAtxg5PLo_XZENWco2hzoDKvpOn7xxF9IC5GgFgPPQmoTeNLd7SEAGltThPNBfzFDHj_VVJ5NmRHp8uEiicIcKyOKhrmYYMDHAaPYxBYaFoJr52c4qzAkUCB0vOtLZSyILdwnAMSTFV0KaXrLznrIINTpag9CIMixNPSPxpqJK1_uUpaIc9YpFE9gCah_OUSRe6t0JBm7N9EmzSPUNYUrJgFxp-4jftU1aevJrWPnZQa9vQJDQF-lkaPSyTaUuWB9EjrAyoKegXACu55yR5u8sHHXPDvH0AfP2mqhAo1B3KA-_S-kPbm_uWSZSS3X2R8RS-gr-OUYPYEL3jVEx9W5b=w494-h421-no"
       >
  </p>
  
  
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/RPNT2WHZAcZDsmpis7BzHmhOA5E1XGBgtCs3hXqJUrHhRJmjwPsE9DhulKU9rn8ZfbICGK0LFfOiDJSp4CnPN60GqOFKxmmzlZEVE75ZLAc9FqmfTbGxvGXMevLctLfXYPCefyuhRZdhLANvjSzb7KzfKfb-i3pDJMbI7ZKlgQfwOo31V2hmZ-5L7_M3pJFaHQgwDnEWbHMgAhtZEZ5kHkKXBZDwMz6MJEJNAaWRhWgOdcQzofyYcJvFSwDyM83a9uQ6OIlKy6ti4mD2xafrKv03FoHcCkeldolWZeek827Kq_JCUx9cvj2bfA8OxCocai0ep415aCDRTF5b1oyvWOUpNnkQkMFiCMbK9zeWnj5fBXxXskSs9Cn037p0tXxFmrzCfn8b9mFnicK18bnRzby_M_fGLv0uSZOxxwjJVH2y7pK4_wQzlRBNJNiYqdoBPifagKaXTv45zVMkzJeg72_Eq8LfNt8BAmHn-wBVoVwDQiZTztxIX32kXUG-uML-6H0nbJperWRenJyMkanetX9z9onUUrAevHnRM1xVog54C7q0VvnD3Ny7JjWrr4JRbdUzpa_5HTOtTgqI5vRgviJ45oyWeHoc0QOHoOzYd1x7mlLC9pFczgufhyvWLStGMZiupDGEhg87UxwDFBgvf3rmB1lscG_1=w748-h319-no"
       >
  </p>
  
## Convolutional block

Tất cả những layer đã trình bày ở trên Convolution, ReLU, Pooling thường sẽ được kết hợp với nhau để tạo thành một Convolutional block. Tùy theo yêu cầu của bài toán, tài nguyên tính toán mà sẽ có nhiều kiến trúc CNN khác nhau gồm một hay nhiều Convolutional block.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/DgeoBnnyrGfwqdB8Q3YfCFTkia9PNIf1NHpM75wWtjkP8h_uzv5Kv2WCSi0IHUaNHQtfm2332JDdCRMvwOr-7P0Ubi6yGpq1nci9a3SFX2kdAZ6OfE6IziZQ4Otji0UNlZMTMGHq_9H1q8Yc-IA77igKdQfidU6dP2-3XiTBSAY0ymAGU-v0Pc3kq54J07QnFRASTSZPs2Sedh2dCRzRnfptD2TsaHyyF4plrfnBq8FjbVhyvgWgNftq0KBXGND0wk_hTP8AZdee2kuoJ2ros7l7Y5Ad7qcdLwR9iQuKZiQ2BYZSRr-dSG5JSE54qSbsniZH_XtBj07Fv3F3u64rvjHmYlezKCG_e18WL-l-7HLsBGs7ElN4hmcfzxdLyaNztbAbLWD9jmMXzXNRUQmGOwHTAcInXdjysmvjh3HgqmT5mEm4Qg1XE1xreZryKSaeUNww1wNs6mDtWFfVQL4Zy3QL0x2tfwieH6B0TvhESAg8yHZgMzqlKli8UA74pz-D9zQHfmC2KnZAxBR4qfX3x0vFV1EwhHq8kivUgD75vWScarkHZIbhCgwpPCwmNkAaXDHcwZ9uzaiwSTUwKH5AQKjx-dxVoN2vl_OUKeHp0ndMB6k6wZyfSsiOYHNzXpsGUiOctv0Bpuq133kVGX2rS1PhAMXGsea4=w401-h219-no"
       >
  </p>

## Fully Connected Layer

Sau bước convolution, ta đã có được một output gồm các feature đã trích ra được từ ảnh gốc. Công việc còn lại đó là dùng những đặc trưng đó để phân loại và đưa ra kết quả.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/wJTm0LRTJmZQMccGS5Vr-jjcWvW_K9q7-Bmy6oAzWS7hI-XPgn3Y11q-msf_DwEMdiNvUEsxfMsiVk3YHNqFuAUnG-QwQ8hZxaTD1vUAZf0Fb5aAElyAvzlZk9LE4AgK3hEfd85eyaZIlSfcy1Y2RBv8MVJwG2CEV_408jmgTMveOyTZzKMoGn1wWNQu_Y-9YT4JYOQ-C6-S09Qh9eHX2u84LUulR9iXCpKQLsJ5hX7Oq69uSDP0DmzIEj1jJECeLczxXdf142nHACGLXZCl_QUgNIb6SyghkkLZmwPEBtzAPDRl2V_IEOh9jof959njq-TKXMzku6m0PMEo3fWh56RNS5GmxPBlM8UEBvteYtwr6VLvAiy-l5MxhWls-EyrEpiphrrbc1tK1Z73DbDcA8oSfSb6_qr8LfgZOEadKrcflJCeY92JPdpfGRZDKSRef2jl_C7AXILyiu-0lBz6EeDdF51YApT2wh4kh8D4Apjy8utmHzdSBZOKZEaStqS0QNq1XKLSlubTolDNEXAxDVRz0TGZEQnVQnH9UGFkYzkXircXKZ98NxmFj2m_Oa69kYHfDxFQl7NAGDlPm9NV2kBFKfunX8FVF71VQUblvPdPnKUHNbKdvJcNyhBynh32qMN1e1UPj6GZHh64_ULvluMEdLsMrN4R=w748-h263-no"
       >
  </p>

Để làm được như vậy, ta flatten data và sử dụng lại mạng Neural Network truyền thống với input là các feature có được từ Convolution layer và output là một softmax layer.

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/kdXwAI6DAgaZkP1I_KAEIosdAluHKUlIhs9ZrhnQ08kE6dfNyGUBwW2r4vTt3S9KiO2CnLvt7ayfTVthYO7BQRS_wlzT2QmKtRJ8d7YzVZkafcF6-XD06W95QJxqYyVVmdbhrKw2mP4LxH3Gr7bk0BmuCpH_YJkyrcxYc892ffEfUiC8EIbmSXTt2nlmmueS6bxPi0zFodM0N7IbdQ-PGvZCJdavKWJXJri0_fBuUm0rAGkBNPHFBBMGrZbnmm3G9wfycrAAR5icSuDfmA2wLS1mBO-QYv03qCYDiYg-VdBGG2hF-mZHinavshr7y_bf7A5ABL4eLE8EzW-3Q5kD4mmW-M4v4yKu-Z97RDEJWI-yIza8PJ6wtUFPhs0FE3rqPGWzdjcuWKXBziwxxZdmpqg3PTPmSRjdqbfxEs42UNbm5Tdl_4o8q0Vj-UKH1YxkCisEM3oo55BxJwUThKY0bia4n3TiR4cAQp9eUHXNH1W5t6CJpu8M72H0zEsoPZU0rmkVNhnjKVIb40PLRj0YNnihuxPqKRCktMMXSNSJXxKno27Hx7jWgHfmusbCXi1YFPx0yq2Ky3aVeY4dVp1iRwGV11lkwlAn_4V2rwFIycf0Roa6gYU3nkY8mFEvEoi06zYNr_fv_zsNQiElvm8ba_ZU9INDljan=w554-h300-no"
       >
  </p>
  
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/a0-mOaZInMgpJSZsL8_WBQ_WjvEAKm4ecuywYPI6Q3hkn0dV4vH4mYdXO85_a0NBb773i1lC3mR95GHVdy-jswHK6Sl5sCQOt6E55577Mdor9xp64YGQKNt-phnInMMPm-VZFxguA9tP11rGWgtT0ZipUCifAyAbJa8YS0O2cPOurcD0foz4X-gi8rDK39dKdYY_FoZexM3CGJ4e8bl0Y6fUdqV5Qx8e0XL0kCjROaXCo4Ml4zF1Gnnd6nVTxIu_uxdn8w6FmT0ZdKYZINXPldOxujgVwrC_yH91P0hGJMJ-FHSuZMVZexIQkdZmloHTKGG4Bi59G3HTpvS4QFhH3eHagSuahxVjjdeeBY1z5-22lLNvSbE6gapP5F7xFkOKdad68ODK5BuW2t2fn8qu40pOaUr6t17qL40NBtTlehBTJD6eQeMLMPr0DNUX_wktwkyWlqszCD-6i35dK3Pr6vDOXjipR01Mw3me2nwfdqVB-EMclAku2HZma5b0xEThH87kwZeqhIFbuxi0Shh2YOJZtE-oisKv3YAYX6WDvadYfg6Dl40Hq8ARy770hveb9Lq2UTdmQZkblX69Anww8J_2WupZIshw7nQpasJDDN-RotZv7YvHh9QJUXngNsmbGAk5y4sFdjqvV_m5uViqXRykc1vDZ1Jz=w484-h152-no"
       >
  </p>
  
## Training

Sau khi đã tìm hiểu về kiến trúc của Convolutional Neural Network. Chúng ta sẽ đến bước training, cũng như nhiều mạng Neural Network khác ở đây ta cũng dùng Backpropagation để training. Tiến trình training có thể thực hiện như sau:
- B1: Khơi tạo tất cả các tham số một cách ngẫu nhiên.
- B2: Nhận ảnh đầu vào thực hiện feed-forward (lan truyền tiến) và tính đầu ra cho softmax-layer. Do các tham số được khởi tạo ngẫu nhiên cho nên xác suất nhận được ở softmax-layer cũng là ngẫu nhiên.
- B3: Tính hàm mất mát (loss function):
$$Total Error = \sum \frac{1}{2} (target probability - output probability)$$
- B4: Dùng backpropagation để tính gradient và update các tham số.
- B5: Lập lại B2 -> B4 với tất cả các training set.

## Ví dụ với Keras và Fashion-MNIST

### Fashion-MNIST
Có lẽ bộ dữ liệu chữ số viết tay MNIST đã quá đổi quen thuộc với chúng ta, đến nỗi nhàm chán. Có nhiều người cho rằng MNIST đã trở nên quá dễ khi có nhiều model đạt độ chính xác gần 99%. Khiến cho việc đánh giá giữa các model trở nên khó khăn khi dùng MNIST. Gần đây cũng đã có một version mới của MNIST đó là [EMNIST](https://www.nist.gov/itl/iad/image-group/emnist-dataset). Trong khi đó, [Zalando Research Team](https://research.zalando.com/) cũng đã public [Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist). Fashion-MNIST gồm 60000 training images và 10000 training images thuộc 10 classes khác nhau: T-shirt/top, trouser, pullover, dress, coat, sandal, shirt, sneaker, bag, ankle boot. Chúng ta hãy thử cài đặt CNN với keras và Fashion-MNIST.

Đầu tiên là import các thư viện cần thiết và thiết đặt các thông số training cơ bản:

```python
import keras
from __future__ import print_function
from keras.datasets import fashion_mnist
from keras.layers import Dense, Flatten
from keras.layers import Conv2D, MaxPooling2D
from keras.models import Sequential
import matplotlib.pylab as plt

batch_size = 128
num_classes = 10
epochs = 10
```
Chuẩn bị dữ liệu: Tất cả các hình ảnh từ Fashion-MNIST đều là các ảnh grayscale có kích thước $28 \times 28$. Fashion-MNIST có thể download trực tiếp từ Keras.

```python
## input image dimensions
img_x, img_y = 28, 28

## load Fashion-MNIST data set directly
(x_train, y_train), (x_test, y_test) = fashion_mnist.load_data()

## reshape the data into a 4D tensor (sample_number, x_img, y_img, num_channels)
## there is only 1 channel in Fashion-MNIST
x_train = x_train.reshape(x_train.shape[0], img_x, img_y, 1)
x_test = x_test.reshape(x_test.shape[0], img_x, img_y, 1)
input_shape = (img_x, img_y, 1)
```

Normalize ma trận pixel về kiểu các số `float32` từ 0 đến 255. Và in ra thông tin của dataset:

```python
## normalize data
x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

# convert class vectors to binary class matrices
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

## print detail of data
print('x_train shape:\t', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')
```

Định nghĩa một CNN model với 2 convolutional layers và một fully connected layer.
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/BFeC6fiSjGFrVXMrc_BfJKWCIKkwnh56FoJsziY8T6h4BPM55TSPjUwxpBomKrDUR49BMfdvVM94OGBq8hXEwAW2Pg_qqv7DzfFCweXkGuwqqcJrmLIZmWa-vk-_6eHAD6_jOc5LPN8cPiy33HWCpEiqvyBl7MLV813TAKJOMsgPMBsJ1aSk3UTAJnEC8NfLyrTfDPu4odStNnuTPkRxu47hFp3lbFD7PKKnZcDcAxL9q580ywan5uBpPYev0QV0P2UVZ2cc93o0Uyb_Co9SVhV6eMQeJMeQlv_6aKOHFmBhzvkDK3SxweXY8uBCDIBio6pIkkifsa26f_sYoqWc9ym9Uqcs0BPXzUDM44_6Ilzevh-Hhw5o12Filmh6bBpbe6pPe0-sgUeLQHqxL0DpRZvvLFT2DITGdkJMhAAUtUpzPkV1hgzybN5eODZ_pw6jAtsK7jBw4WpAfLTGYqlWANzU9bMXjt2GUqOuOzvD8IsUWflRAI57ef9GWnH0WgIifMLYaX9ncXiNdYET7wm1u4bHew5sbpzsoQ8_VyCrI-A3geEgQJbDjenavKpNY1psqHypQhpqBJk5hWAy_1LHWcZIe3lTzsT69xZJYIPVtwKjKBPVejxWGkifkkS0bD6M12zsJzMzHEYtvNFyez5kC-dpdZUTqh3W=w1725-h572-no"
       >
  </p>

```python
# define CNN (LeNet)
model = Sequential()

## layer 1
model.add(Conv2D(32, kernel_size = (5, 5), strides = (1, 1),
    activation = 'relu', input_shape = input_shape))
model.add(MaxPooling2D(pool_size = (2, 2), strides = (2, 2)))

## layer 2
model.add(Conv2D(64, (5, 5), activation = 'relu'))
model.add(MaxPooling2D(pool_size = (2, 2)))

## FC layer
model.add(Flatten())
model.add(Dense(1000, activation = 'relu'))
model.add(Dense(num_classes, activation = 'softmax'))
```

Định nghĩa hàm mất mát cross-entropy và dùng Adam optimizer, ngoài Adam chúng ta cũng có thể dùng SGD (Stochastic gradient descent).

```python
model.compile(loss = keras.losses.categorical_crossentropy, 
        optimizer = keras.optimizers.Adam(), metrics = ['accuracy'])
```

Tiếp theo là bước training model:

```python
# training model
model.fit(x_train, y_train, batch_size = batch_size,
        epochs = epochs,
        verbose = 1,
        validation_data = (x_test, y_test),
        callbacks = [history])
```
Test model
```python
# logging metrics
class AccuracyHistory(keras.callbacks.Callback):
    def on_train_begin(self, logs = {}):
        self.acc = []

    def on_epoch_end(self, batch, logs = {}):
        self.acc.append(logs.get('acc'))

history = AccuracyHistory()

# test model
score = model.evaluate(x_test, y_test, verbose=0)

# show performance
print('Test loss:', score[0])
print('Test accuracy:', score[1])

plt.plot(range(1, 11), history.acc)
plt.xlabel('Epochs')
plt.ylabel('Accuracy')
plt.show()
```
Kết quả cho thấy mô hình đạt độ chính xác khá cao chỉ sau 10 lần training (91.66%). Ta có thể so sánh với một số classifier khác tại [Fashion-MNIST Benchmark](http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/)

<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/2l2jM92Rs6sr8_5KRRZ_Vil8xfr8bTBseJKArBrSrdLYoI6ITHYAOO1Z-6xIEu-HPoPqCSFFJTIu2j9MnD0ElSKkte97ZrRlB6nTJE6VcZbZPzQqeO3qjTM5Fyw2uPQ4sdcBHkMNOHh18kcSCBEQPhL6wVPFKC76DL6rvhPA6M_DtkpFmZHeqopUVeOERcJhDsvV9Ha6eKXUasmfNkxpq9XU79MoNe-qdxz7bPF8XAa7mUX80qtWwPZLttdluoB9KnL4RhFnBJwyEdlMf6BCb_Dn6vatve22iNQCVaX98-SgdMJtdn0HSZ93SctdI_e9M6vIRPwrSElxeCYLg2gz6T6oQbU4QBR6Y_jcYckM-cjZY6UVSqJrd3yDRpV1DiuN582Gzv0KH7bwwYO2RTRhx16_y6vFhph-BD6S9axh8yO0UOyvHmghH4u6isOOsHbumU4J7GUJVK-nFg7lW_9ksjlQ4vTiDOW9bXTD7JJkZk7Ld2iKyRah7R6eJ0YLZXfJWgtkTcqaNjRywFblaLEoaHC0FceuR729gceguHLtSWpGHJNGEnfI6hSzc8FNCTLQlq_lC8LrwVK3px51hFrDbamCojyC8xErqSS-kY_IlPhMOzIQDeqQMt7db5A9kkmmI8bXwxOrDIxT3jqzzEmIc5NArRBrVwA-=w640-h480-no"
       >
  </p>
  
<p align = "center">
  <img
       src = "https://lh3.googleusercontent.com/4-xzGP56vXcsEGyca6MC1u2ZMq-_s-mXTMlWmRT5HLaBiiJ_TgxWcaxRMRu39gwGKxufuc830pX8zqBJ7zerceIOCPVKXFaIZ_YKg8wMohUBWFScMQeAHWBY4w5443TQGMVBjgNj1zJz3ZdLLBKQzzuSdrwtZLhwpOZGHC0R9w0iQyoZg4G2WHQuDsPW4BklI4XexItA3zBj3UG7w2ZQCdkGfb-gaHsQVqPkg0jfzSH-GUn-VZwmygDk6GQaoZWXW1XgnwAFWyAWoEFr8v21nUvZFp5TSF0XudlHWM9Cegh-1SNecQCcgzEcOyVf2AE42EGpFZxLHYgv-cN3Na7QkGSu90cxeQOloOZeBC4Y3JO_0gbN7nh1dX2aBwJaiApKzn0Poh-cpwFVLFj7WeNvqgJoS8FU_pMB31LKvequcVBGE-71EatJCCGPecHHaws9U_h1Zgp1vXLz4g33b7NN1ca4K7mJqDzLOLf8fWND9hYFAazFz8XK3uTdra0_RzYcP8tRxf01Jyrq2dO8-AqsqwQvXgYBQdguWgObQxFkEumcOSDKO7SFgV8NNkp_GMHTMuyvzNS5C05_VUWffj5-FsCTmRjt2PKneUoSsZ_lcQM0pBjvH7FW3_UViRulZsXpJCPpvXJbaz26sD0qabTjnRVWtnD0a0a5=w652-h386-no"
       >
  </p>
  
## Source code

- [Keras CNN](https://gitlab.com/hdi-superuser/deep-learning/blob/master/keras_cnn.py)

## Tài liệu tham khảo
- [An Intuitive Explanation of Convolutional Neural Networks](https://ujjwalkarn.me/2016/08/11/intuitive-explanation-convnets/)

- [CS231n Convolutional Neural Networks for Visual Recognition: ConvNets Note](http://cs231n.github.io/convolutional-networks/)

- [Understanding of Convolutional Neural Network (CNN) — Deep Learning](https://medium.com/@RaghavPrabhu/understanding-of-convolutional-neural-network-cnn-deep-learning-99760835f148)

- [A Beginner's Guide To Understanding Convolutional Neural Networks](https://adeshpande3.github.io/A-Beginner%27s-Guide-To-Understanding-Convolutional-Neural-Networks/)

- [Keras tutorial – build a convolutional neural network in 11 lines](http://adventuresinmachinelearning.com/keras-tutorial-cnn-11-lines/)
