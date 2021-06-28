# Cài đặt Webpack và các loader
    yarn add webpack webpack-cli webpack-dev-server style-loader css-loader sass sass-loader file-loader typescript ts-loader -D
###### Giải thích
  -  ***webpack*** là phần lõi của webpack
  -  ***webpack-cli*** giúp ta gõ được lệnh của webpack trên terminal (gián tiếp thông qua file package.json)
  -  ***webpack-dev-server*** hỗ trợ tạo một server localhost cho môi trường dev
  -  ***style-loader, css-loader*** giúp bạn có thể import được css vào file js
  -  ***sass, sass-loader*** giúp bạn biên dịch scss sang css
  -  ***file-loader*** giúp bạn import được các file ví dụ như ảnh, video vào file js
  -  ***typescript***: Phần lõi của ngôn ngữ Typescript
  -  ***ts-loader***: Giúp tích hợp Typescript vào webpack

# Cài đặt một số plugin bổ trợ webpack
    yarn add clean-webpack-plugin compression-webpack-plugin copy-webpack-plugin dotenv-webpack html-webpack-plugin mini-css-extract-plugin webpack-bundle-analyzer -D

###### Giải thích
   - ***clean-webpack-plugin***: Giúp dọn dẹp thư mục build trước khi build webpack. Ví dụ thư mục build của bạn đang chứa bản build trước, bây giờ bạn build lại thì plugin này sẽ xóa bản build trước.
   - ***compression-webpack-plugin***: Giúp bạn nén các file css, js, html… thành ***gzip***
   - ***copy-webpack-plugin***: Giúp bạn copy các file ở thư mục dev vào thư mục build. Ví dụ bạn có các file như favicon.ico, robots.txt cùng cấp với index.html, bạn muốn khi build xong thì các file này cũng có mặt ở bản build. Nếu không có plugin này thì bạn phải copy thủ công.
   - ***dotenv-webpack***: Giúp bạn dùng được các biến môi trường ở file ***.env*** và trong app của bạn
   - ***html-webpack-plugin***: Giúp clone ra 1 file index.html từ file html ban đầu. Tại sao lại cần clone thì bạn có thể tham khảo bài Webpack
   - ***mini-css-extract-plugin***: Bình thường thì css sẽ nằm trong file js sau khi build. Và khi chạy app thì js sẽ thêm các đoạn css đó vào thẻ <code><style></style><code>. Bây giờ mình không muốn như vậy, mình muốn css phải nằm ở file riêng biệt với js và khi chạy app thì js sẽ tự import bằng thẻ <code><link><code>. Đó là chức năng của plugin này
   - ***webpack-bundle-analyzer***: Giúp bạn phân tích bản build, coi thử thư viện nào đang chiếm bao nhiêu % bản build,…