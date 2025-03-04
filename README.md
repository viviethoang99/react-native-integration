1, Cài đặt
npm:
```Bash
npm install react-native-whitelabel-library
```
yarn:
```Bash
yarn add react-native-whitelabel-library
```
2, IOS
* Podfile
```
plugin 'cocoapods-user-defined-build-types' # <- ADD THIS LINE
enable_user_defined_build_types! # <- ADD THIS LINE

target 'ExampleApp' do
  ...

# ADD THIS POD:
  pod 'NPayFrameworkMC',:build_type => :dynamic_framework,
                        :git => 'https://github.com/viviethoang99/ios_sdk.git',
                        :tag => '2.0.46',
                        :modular_headers => true

  post_install do |installer|
    installer.pods_project.targets.each do |target|
      if ['CryptoSwift', 'KeychainAccess', 'lottie-ios'].include?(target.name)
        target.build_configurations.each do |config|
          config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '12.0'
        end
      end
    end
  end
```

* Info.plist
Cấu hình quyền sử dụng Camera (dùng cho chức năng eKYC) trong Info.plist
```Yaml
<key>NSCameraUsageDescription</key>
<string>Cho phép ứng dụng truy cập máy ảnh, để có thể chụp hình khi xác thực tài khoản và quét mã vạch thanh toán</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Cho phép ứng dụng ảnh để chọn ảnh mã QR hoặc chụp giấy tờ xác thực</string>
```
## Usage
Khởi tạo thư viện:
```Typescript
import { NinepaySdkWrapper } from 'react-native-whitelabel-library';

const config = {
   merchantCode: merchantCode,
   secretKey: secretKey,
   uId: uId,
   colorCode: '15AE62',
   flavorEnv: env,
   phoneNumber: phoneNumber,
 };

sdkInstance.current = NinepaySdkWrapper.init(config);
```

**Constant:**
|Params|Chú thích|
|---|---|
|merchantCode|Đây là mã của đối tác khi tích hợp SDK|
|secretKey|Đây là khoá để kết nối API với 9pay|
|uid|Định danh người dùng sử dụng tài khoản của merchant, không bắt buộc, có thể truyền null|
|env|Môi trường sử dụng, có 2 môi trường là staging (môi trường test code) và production (môi trường chạy thực tế), xem bảng dưới để biết cấu hình.|
|brandColor|Mã màu mà merchant muốn sử dụng ở các button trong SDK, mã màu hex và không chứa ký tự #, ví dụ 15AE62|
|phoneNumber|SDK yêu cầu cung cấp số điện thoại hợp lệ theo định dạng của Việt Nam để có thể sử dụng|
**Bảng hằng số sử dụng cấu hình trong hàm SDK**

