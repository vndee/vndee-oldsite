---
published: true
title: Cài đặt Pytorch trên Ubuntu 18.04
date: '2018-09-21 02:57:18 +0700'
comments: true
categories:
  - deep-learning
tags: deep-learning
author: Duy V. Huynh
lang: VI
description: >-
  Cài đặt môi trường lập trình deep learning và pytorch framework cho máy chạy
  Ubuntu 18.04 và không có GPU.
mathjax: true
---

![Pytorch](https://discuss.pytorch.org/uploads/default/original/2X/3/35226d9fbc661ced1c5d17e374638389178c3176.png)

Pytorch là một deep learning platform mã nguồn mỡ được phát triển bởi facebook và các trường đại học lớn.  
Để cài đặt pytorch trên hệ điều hành Ubuntu 18.04 không GPU thì có rất nhiều cách. Tuy nhiên phổ biến nhất vẫn là dùng Anaconda.

### Cài đặt Anaconda
Try cập vào trang chủ của [Anaconda](https://www.anaconda.com/download/#download) để download phiên bản phù hợp. Tải về file có dạng Anaconda3-5.2.0-Linux-x86_64.sh. Sau đó chuyển vào thư mục chứa file vừa download và thực hiện câu lệnh:  
```shell
sudo chmod +x Anaconda3-5.2.0-Linux-x86_64.sh  
./Anaconda3-5.2.0-Linux-x86_64.sh
```
Sau đó add path variable của Anaconda bằng cách:
```shell
export PATH=~/anaconda3/bin:$PATH
```
### Cài đặt Pytorch  
Ta thực hiện cài đặt pytorch thông qua Anaconda bằng các câu lệnh sau:  
```shell
conda install pytorch-cpu torchvision-cpu -c pytorch
```
### Cài đặt Jupyter Notebook
```shell
conda install -y -q -c numpy matplotlib jupyter nb_conda
```
