Bài toán nhận diện cảm xúc qua hình ảnh khuôn mặt

Các bước thực hiện :

Bước 1: Thu Thập Dữ Liệu và đọc ảnh

-Dữ liệu được lấy từ trang web Kaggle gồm các hình ảnh đã được phân loại
https://www.kaggle.com/datasets/msambare/fer2013
![image](https://github.com/user-attachments/assets/3268019f-d8f3-4499-8af2-4af805cf9c16)

 
Đọc và hiển thị ảnh: Sử dụng thư viện OpenCV để đọc ảnh từ thư mục chứa dữ liệu huấn luyện. Hiển thị ảnh bằng Matplotlib. 
![image](https://github.com/user-attachments/assets/d21727a4-a9fb-4a6f-bbe0-ea5d4180e88e)
![image](https://github.com/user-attachments/assets/0545c39a-83c1-40e1-97dd-8001e2dea798)

   
Tạo danh sách lớp: Tạo danh sách các lớp cảm xúc (angry ,disgust,fear, happy,neutral,sad,supriesed tương ứng "0", "1", "2", "3", "4", "5", "6" bằng cách đổi tên thư mục).
![image](https://github.com/user-attachments/assets/52ea8c1a-86b0-4763-a232-f2fe243ce27a)

 
Thay đổi kích thước ảnh: Tất cả các ảnh được thay đổi kích thước thành 224x224 để phù hợp với mô hình MobileNetV2.
![image](https://github.com/user-attachments/assets/db409ee7-e0ae-40b2-b615-d0c2b1d836b2)

 
Bước 2.Tạo dữ liệu huấn luyên

1.Tạo mảng dữ liệu huấn luyện: Duyệt qua từng lớp, đọc từng ảnh, thay đổi kích thước và lưu vào mảng dữ liệu huấn luyện.
![image](https://github.com/user-attachments/assets/2b5d6f0a-980f-465c-88cd-7be8102281ef)

 
2.Shuffle dữ liệu: Sử dụng hàm random.shuffle để trộn lẫn dữ liệu, giúp cải thiện quá trình huấn luyện.
![image](https://github.com/user-attachments/assets/74aa0a32-d778-4bdd-996d-1e33d1b6b623)

 
3.Tách đặc trưng và nhãn: Tách mảng dữ liệu huấn luyện thành hai phần: X (dữ liệu ảnh) và y (nhãn tương ứng).
![image](https://github.com/user-attachments/assets/fd450361-7c3c-45f9-9c61-99a7db4d0f33)

 
Bước 3: Chuẩn bị mô hình 

 1.Chuyển đổi và chuẩn hóa dữ liệu: Chuyển đổi X thành dạng numpy array và chuẩn hóa pixel
![image](https://github.com/user-attachments/assets/7fe284ca-8976-4979-9fc6-d931a574967b)

 
2.Sử dụng mô hình MobileNetV2: Tải mô hình MobileNetV2 đã được huấn luyện trước và sử dụng làm mô hình cơ sở.
![image](https://github.com/user-attachments/assets/fd53ee15-0e3a-4cc4-b737-f57599279143)

 
3.Đóng băng các lớp của mô hình cơ sở: Để không cho phép thay đổi trọng số của các lớp này trong quá trình huấn luyện.
![image](https://github.com/user-attachments/assets/3a9e3686-bf66-49e7-a665-d7b563d11717)

 
Bước 4: Xây dựng mô hình mới

1.Thêm các lớp mới: Thêm các lớp Dense và Activation vào mô hình để phù hợp với bài toán nhận diện cảm xúc (7 lớp đầu ra).
![image](https://github.com/user-attachments/assets/2e0771ab-42bf-48b6-946f-df11282ac8fd)

 
2.Tạo mô hình hoàn chỉnh: Tạo mô hình mới từ đầu vào của MobileNetV2 và đầu ra từ lớp Dense cuối cùng.
![image](https://github.com/user-attachments/assets/6100342b-c5ce-4a27-823c-8add46e58e74)

 
3.Compile mô hình: Sử dụng hàm mất mát "sparse_categorical_crossentropy", bộ tối ưu hóa "adam", và đo lường độ chính xác.
![image](https://github.com/user-attachments/assets/aad05dd0-9fc2-4b1d-ac7d-e33d29677747)

 

Bước 5: Huấn luyện mô hình 

1.Huấn luyện: Huấn luyện mô hình trên dữ liệu X và Y với 15 epoch.
càng nhiều epoch, mô hình càng chính xác, ở đây với epoch 15, độ chính xác là 71%
![image](https://github.com/user-attachments/assets/b37f16b7-3fdf-4226-ad49-2ba8c8f13d29)

 
2.Lưu mô hình: Lưu mô hình đã được huấn luyện để sử dụng sau này.
 
Bước 6: Nhận diện cảm xúc từ ảnh mới

1.Đọc ảnh: Đọc ảnh mới bằng OpenCV.
 
2.Phát hiện khuôn mặt: Sử dụng Haar Cascade để phát hiện khuôn mặt trong ảnh.
![image](https://github.com/user-attachments/assets/dce26a44-74f6-4043-aa7a-2c58fd13a6c2)
![image](https://github.com/user-attachments/assets/9a49e7b5-2e1c-410a-8d14-1cd6cb667e8c)
![image](https://github.com/user-attachments/assets/deea426a-3092-4705-b9a5-3b2029940109)

 
 
3.Cắt và chuẩn bị ảnh khuôn mặt: Cắt phần khuôn mặt từ ảnh gốc và thay đổi kích thước thành 224x224. Chuẩn hóa giá trị pixel.
   ![image](https://github.com/user-attachments/assets/589a4b6b-07e5-485f-8ce4-21be4ecb4d6f)

4.Dự đoán cảm xúc: Sử dụng mô hình đã huấn luyện để dự đoán cảm xúc từ ảnh khuôn mặt. Hiển thị kết quả dự đoán.
![image](https://github.com/user-attachments/assets/773b2f71-f84d-4482-ae89-8467764bb0da)

 
	Kết quả của khuôn mặt dự đoán: neutral : chính xác












Chương 5.Kết luận

-Mô hình Random Forest đã giúp dự đoán giá vàng với độ chính xác cao. Qua các bước từ xử lý dữ liệu, xây dựng mô hình đến đánh giá và trực quan hóa kết quả, mô hình đã cho thấy khả năng dự đoán hiệu quả.
-Mô hình nhận diện cảm xúc khuôn mặt đã dự đoán khá chính xác, tuy nhiên qua kiểm thử nhiều ảnh, nó vẫn nhầm lẫn một số ảnh có cảm xúc khó.
Ví dụ: cảm xúc của khuôn mặt này là sad, nhưng khi test thì trả về kết quả là happy
 
-Hướng phát triển trong tương lai: Để cải thiện mô hình nhận diện cảm xúc khuôn mặt, tương lai có thể sử dụng nhiều data hơn, training với hệ số epoch nhiều hơn để có độ chính xác cao nhất có thể, tuy nhiên sẽ tốn khá nhiều thời gian và cần cấu hình máy khỏe hơn để huấn luyện

