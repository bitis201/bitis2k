#Tìm hiểu về Linux kernel

 ##1.Hướng dẫn sử dụng
 
  a.Chọn thư mục chứa mã nguồn,chuột phải chọn Open In Terminal để mở terminal.
  
  b.Ta gõ lệnh make hoặc make all, tiến trình ‘make’ sẽ dựa vào Makefile và Kbuild để biên dịch mã nguồn, tạo ra kernel module.
  
  ![Capture](https://user-images.githubusercontent.com/88387501/129376371-589747fe-5f7e-4a7d-af74-9be5b66f1b3b.PNG)
  
  c.Ta gõ lệnh modinfo garn.ko để xem thông tin của module.
  
  ![Capture1](https://user-images.githubusercontent.com/88387501/129376235-af6025d7-0b68-49d2-85ac-c2896b690d8d.PNG)
  
  d.Gõ lệnh sudo insmod garn.ko để để lắp module vào trong kernel.
  
  ![Capture2](https://user-images.githubusercontent.com/88387501/129376402-8ba31055-4ddd-48d1-8de7-395c77ef4bab.PNG)
  
  e.Gõ lệnh lsmod | grep garn để kiểm tra.
  
  ![3](https://user-images.githubusercontent.com/88387501/129375772-244d931a-a0d9-49f5-955e-5545ddc28f4a.PNG)
  
  f.Gõ lệnh sudo ./user_test để lấy ngẫu nhiên một số nguyên không âm.
  
  ![4](https://user-images.githubusercontent.com/88387501/129377070-0802ebd9-549b-4a64-9efa-4dfd94caa984.PNG)
  
  g.Gõ lệnh sudo rmmod garn để tháo module ra khỏi kernel.
  
  ![5](https://user-images.githubusercontent.com/88387501/129377075-c9fb0e9f-c27e-47bd-9600-e323785b6fc0.PNG)
  
  h.Gõ lệnh dmesg để xem quá trình hoạt động của kernel kể từ khi nó khởi động.
  
  ![6](https://user-images.githubusercontent.com/88387501/129377078-67b8e478-98a0-4f59-8890-a52f8a5c5d3b.PNG)
  
  i.Gõ lệnh make clean để dọn sạch các file được tạo ra trong quá trình biên dịch.
  
  ![7](https://user-images.githubusercontent.com/88387501/129377081-b07b1d9d-2a15-481f-a1cf-cde6d6334344.PNG)
  
   ##2.Giải thích mã nguồn:
   
    • Garn.c: Chứa mã nguồn để biên dịch và tạo ra một kernel module. Module này tạo ra một character device có chức năng sinh các số       ngẫu nhiên và cho phép các tiến trình ở user space đọc các số ngẫu nhiên đó.

    •	static int my_open(struct inode *i, struct file *f); :Hàm thực hiện thao tác mở file thiết bị.

    •	static ssize_t my_read(struct file *f, char __user *buf, size_t len, loff_t *off); : đọc một số ngẫu nhiên là số nguyên không         âm.

    •	get_random_bytes(&RandomNum,sizeof(RandomNum))  : hàm tạo ra một số nguyên bên trong linux kernel.

    •	copy_to_user(buf,&RandomNum,4)  : hàm dùng để sao chép dữ liệu từ kernel buffer vào user buffer.

    •	static int __init garn_init(void):  hàm khởi tạo bao gồm các thành phần như:

        o	Yêu cầu kernel cấp phát device number.

        o	Yêu cầu kernel tạo device file.

        o	Đăng ký các hàm entry point với kernel ( my_open, my_close, my_read).

    •	Cấu trúc file operations: trong đó các thuộc tính sẽ nhận giá trị là tên các hàm sẽ được thực hiện tương  ứng với các thao tác       mở,đóng, đọc.

    •	static void __exit garn_exit(void): Hàm thực hiện công việc hủy đăng ký, gỡ bỏ module driver

    •	Makefile, Kbuild: Để biên dịch kernel module, ta sử dụng phương pháp Kbuild. Theo phương pháp này, chúng ta cần tạo ra 2 file:        một file có tên là Makefile, file còn lại có tên là Kbuild.
  
  
