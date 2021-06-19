# VietQR API

VietQR là dịch vụ thanh toán, chuyển khoản bằng mã QR được xử lý qua mạng lưới Napas. 
VietQR.io cung cấp giải pháp tạo mã QR nhanh chóng thông qua API.

## Bắt đầu nào ...

Dùng CURL gọi API để tạo mã: 
```bash
curl --location --request POST 'https://api.vietqr.io/v1/generate' \
--header 'x-api-key: we-l0v3-v1et-qr' \
--header 'Content-Type: application/json' \
--data-raw '{
    "accountNo": "113366668888",
    "accountName": "QUY VAC XIN PHONG CHONG COVID 19",
    "acqId": "970415",
    "addInfo": "Ung Ho Quy Vac Xin",
    "amount": "79000",
    "format": "vietqr_net"
}'
```
Nhận được phản hồi JSON như sau: 
```json
{
    "code": "00",
    "desc": "Gen VietQR successful!",
    "data": {
        "acqId": "970415",
        "accountName": "QUY VAC XIN PHONG CHONG COVID 19",
        "qrDataURL": "data:image/png;base64,QRCODE_BASE_64_HERE"
    }
}
```
Thử kết quả bằng cách copy trường qrDataURL paste lên trình duyệt để thấy ngay kết quả hình ảnh mã QR đã được tạo ra: 


![alt text](https://gblobscdn.gitbook.com/assets%2F-McEokkaiaWK1lEl6XEh%2F-McGaExfH3lKGWKR6SRe%2F-McGdISM7lFY68WEi86l%2Fvietqr-io-api-gen-qr-code.png?alt=media&token=20211139-88ba-429a-a31d-f7d703f139a5).

## API Lấy danh sách mã ngân hàng
Api trả về danh sách các ngân hàng mà VietQR hỗ trợ. Dùng API để tra cứu mã số ngân hàng trước, sau khi có giá trị mã ngân hàng mới dùng API Tạo Mã QR được.
```
GET  https://api.vietqr.io/v1/banks
```
[Test thử trực tiếp](https://api.vietqr.io/v1/banks)

Kết quả trả về: 
```json
{
    "code": "00",
    "desc": "Get Bank list successful! Total 50 banks",
    "data": [
        {
            "code": "ACB",
            "name": "Ngân hàng TMCP Á Châu",
            "bin": "970416",
            "short_name": "ACB",
            "logo": "https://vietqr.net/img/ACB.6e7fe025.png",
            "vietqr": 3
        },
        {
            "code": "VPB",
            "name": "Ngân hàng TMCP Việt Nam Thịnh Vượng",
            "bin": "970432",
            "short_name": "VPBank",
            "logo": "https://vietqr.net/img/VPB.ca2e7350.png",
            "vietqr": 3
        },
    ]
}
```

Ý nghĩa các trường : 
- bin : Mã định danh ngân hàng (dùng trong API tạo mã QR)
- code : Tên mã ngân hàng (Napas quy định, thường 3 kí tự)
- short_name : Tên ngắn gọn thường gọi của ngân hàng
- name : Tên đầy đủ của ngân hàng
- logo : Logo ngân hàng (Nguồn: VietQR.NET)
- vietqr : Mức độ hỗ trợ VietQR của ngân hàng

Các giá trị của trường `vietqr`
- vietqr = 0 : Ngân hàng chỉ hỗ trợ tạo mã, chưa hỗ trợ  quét mã
- vietqr = 1 : Ngân hàng tuyên bố hỗ tích hợp quét mã trong App
- vietqr = 2 : Ngân hàng đã hỗ trợ quét mã 1 phần trong App
- vietqr = 3 : NGân hàng đã hỗ trợ hoàn toàn mã vietqr trong App
