
# Tool kiểm tra tx ví (worker)

Vì trên web `explorer` chưa hiển thị `tx` của ví nên dùng tool này kiểm tra các `tx` của ví (`worker`).

## Tool kiểm tra tx ví (worker):

http://worker-tx.nodium.xyz/

### Xem các tx của ví (worker):

Nhập địa chỉ ví vào ô `Wallet Address`

`Order by` là `Newest First` để xắp xếp tx mới nhất lên trên.

Nhấn `Search` để tìm các tx của địa chỉ đó. Kiểm tra tx ví (worker)

![image](https://github.com/user-attachments/assets/bec80655-2e9b-421c-a659-5324be81c6f3)


Kết quả ở cột `Action` là `MsgRegisterWorker` là tx của worker đăng ký topic.

Kết quả ở cột `Action` là `MsgInsertBulkWorkerPayload` là tx của worker leader gởi giá dự đoán vào mạng Allora.

### Xem các tx của worker đã hoạt động trên mạng:

`Order by` là `Newest First` để xắp xếp tx mới nhất lên trên.

Chọn: `Action` là `MsgRegisterWorker` là tx của worker đăng ký topic.

Chọn: `Action` là `MsgInsertBulkWorkerPayload` là tx của worker leader gởi giá dự đoán vào mạng Allora.

![image](https://github.com/user-attachments/assets/9be4d22a-41d3-4f27-84da-f3bd619ec6a3)

_Kiểm tra tx worker_
_
Kích vào liên kết TX để xem chi tiết tx.


### Thông tin thêm:

Ví worker không có tx `MsgInsertBulkWorkerPayload` chưa chắc `worker` lỗi, vì chỉ cần ví đó có trong danh sách `Worker Data Bundles` trong tx của ví khác là chứng tỏ `worker` đó đã hoạt động. Như hình dưới: `Worker Data Bundles`
![image](https://github.com/user-attachments/assets/76f543f6-1fb2-4f63-9c96-9063abad6800)

Ví có tx `MsgInsertBulkWorkerPayload` nhưng không có trong danh sách `Worker Data Bundles` của chính tx mình gởi thì cần kiểm tra lại logs có bị lỗi dự đoán hay gì không. Như hình dưới: `Worker Data Bundles`
![image](https://github.com/user-attachments/assets/487734f8-67d8-4e9f-93a5-e54ca2645915)
