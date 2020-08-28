# Attendance Checking Face Recognition

Ghi nhận điểm danh thông qua nhận dạng khuôn mặt 

Recognize and manipulate faces from Python or from the command line with
the world's simplest face recognition library.

Được xây dựng bằng tính năng nhận diện khuôn mặt của [face_recognition](http://https://github.com/ageitgey/face_recognition/)

## Đội nhóm
Dương Quốc Nghi - duongquocnghi2016@gmail.com

## Mô tả
Thực hiện việc ghi nhận chấm công khi nhân viện đi tới văn phòng làm việc đơn giản và hiệu quả hơn so với vân tay hoặc ghi nhận tay.

## Dữ liệu
Dữ liệu sẻ được chủ động ghi nhận thông qua camera khi chạy chức năng đăng ký khuôn mặt
![]()

## Tính năng

#### Đăng ký khuôn mặt

Bắt đầu thực hiện đăng ký khuôn mặt với 30 tắm hình và hệ thống sẻ tự động chọn và chụp các tắm hình khác nhau

<!-- ![](/Users/admin/Job/attendance_checking_face_recognition/image_demo/dang_ky_khuon_mat.png) -->

Chụp hình đạt 100% để hoàn tất quá trình đăng ký

<!-- ![](/Users/admin/Job/attendance_checking_face_recognition/image_demo/dang_ky_khuon_mat_100.png) -->

```python
import matplotlib.pylab as plt

data_face = [] 
data_face_encodings = []
data_face_names = []
name = 'Nghi'
path_name = 'nghi/'

start_input() 
label_html = 'Capturing...'
img_data = ''
max_count = 30
min_face_distances = 0.3

while True:
  
    js_reply = take_photo(label_html, img_data)  
    if not js_reply:
      known_face.extend(data_face)
      known_face_encodings.extend(data_face_encodings)
      known_face_names.extend(data_face_names)

      print(name)
      cols = 4
      rows = 2
      fig, axs = plt.subplots(rows,cols,figsize=(6 * cols - 1, 5 * rows - 1))
      for i in range(rows):
        for j in range(cols):
          target = np.random.choice(len(data_face))
          axs[i][j].grid('off')
          axs[i][j].axis('off')
          axs[i][j].imshow(np.squeeze(data_face[target]))

      plt.show()

      break

    frame, image_byte = js_reply_to_image(js_reply) 

    drawing_array, faces_found_number, face_encodings = find_face(
        frame, 
        js_reply['videoWidth'], 
        js_reply['videoHeight'], 
        '{}%-{}'.format(int(len(data_face) / max_count * 100), len(data_face)))
        
    if len(face_encodings) == 1:
      
      face_distances = [0]
      if len(data_face_encodings) > 0:
        face_distances = face_recognition.face_distance(data_face_encodings, face_encodings[0]) 

      if (min(face_distances) > min_face_distances or len(data_face_encodings) == 0) and len(data_face_encodings) < max_count:
        data_face_names.append(name)
        data_face_encodings.append(face_encodings[0])
        data_face.append(frame)
        save_image('{}{}_{}.jpg'.format(path_name,name,len(data_face_encodings)),image_byte)

    img_data = drawing_array_to_bytes(drawing_array) 
```

#### Nhận diện khuôn mặt

Chạy nhận diên để phát hiện khuôn mặt và ghi nhận thông tin

<!-- ![](/Users/admin/Job/attendance_checking_face_recognition/image_demo/nhan_dien_khuon_mat.png) -->

```python
start_input()  
label_html = 'Capturing...'
img_data = ''
count = 0 

while True:
  
    # lấy hình tư webcame 
    js_reply = take_photo(label_html, img_data)           
    if not js_reply:
        break

    # chuyển hình từ base64 to np.array
    frame, image_byte = js_reply_to_image(js_reply)

    drawing_array, nameUser, accuracy = check_face(frame, js_reply['videoWidth'], js_reply['videoHeight']) 

    img_data = drawing_array_to_bytes(drawing_array)   
```

Nhận diện thành công tiến hành ghi nhận thông tin

Ghi nhận vào file sheet trên google drive
<!-- ![](/Users/admin/Job/attendance_checking_face_recognition/image_demo/ghi_nhan_thong_tin_tren_sheet.png) -->
Lưu lại hình ảnh lúc nhận diện thành công
<!-- ![](/Users/admin/Job/attendance_checking_face_recognition/image_demo/ghi_nhan_hinh_anh_checking.png) -->

```python
    drawing_array, nameUser, accuracy = check_face(frame, js_reply['videoWidth'], js_reply['videoHeight'])   

    if nameUser != 'Unknown' and nameUser not in recorded:
      recorded.append(nameUser)
      setCheckIn(nameUser,image_byte, accuracy) 
```

## Thanks

* Cảm ơn [ageitgey](https://https://github.com/ageitgey) đã tạo ra face_recognition, có các ví dụ cụ thể và cung cấp các hàm gọi tiện ích.
* Cảm ơn tất cả những người làm việc trên tất cả các thư viện khoa học dữ liệu tuyệt vời của Python như numpy, scipy, scikit-image, gối, v.v. đã làm cho loại nội dung này trở nên dễ dàng và thú vị trong Python.
* Cảm ơn COTAI và VTC Academy đã tạo ra các khóa học hiệu quả, cơ bản đến chuyên sâu về AI