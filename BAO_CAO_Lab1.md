# Lab 1 — Bước 4: So sánh DevOps vs Truyền thống

## 4.1 Bảng so sánh

| Tiêu chí | Cách THỦ CÔNG (Traditional) | Cách DEVOPS (CI/CD Pipeline) |
|----------|---------------------------|------------------------------|
| **Làm sao để deploy?** | Copy file bằng tay lên server (FTP/SCP), đăng nhập server, cấu hình web server, khởi động lại dịch vụ | Push code lên GitHub → pipeline tự chạy |
| **Mất bao lâu?** | 15–30 phút mỗi lần, lâu hơn nếu gặp lỗi | ~10 giây |
| **Ai làm deploy?** | Developer hoặc người vận hành (Ops) làm bằng tay | GitHub Actions (máy tự làm) |
| **Có bị sai sót không?** | Dễ sai: quên file, copy nhầm phiên bản, sai cấu hình, mỗi người làm một kiểu | Không — pipeline nhất quán 100% |
| **Rollback nếu lỗi?** | Phải tìm lại bản backup cũ (nếu có) rồi upload lại bằng tay, chậm và rủi ro | `git revert` + push → tự động rollback |
| **Làm sao biết deploy thành công?** | Tự mở trình duyệt kiểm tra, không có log hay thông báo chung | GitHub Actions gửi notification ✅/❌ |

## 4.2 DevOps workflow đã trải nghiệm

```
 PLAN          CODE               BUILD / TEST                 DEPLOY                 MONITOR
┌──────┐    ┌───────────┐    ┌──────────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ Lab  │ →  │ index.html│ →  │ GitHub Actions       │ →  │ peaceiris/       │ →  │ Mở *.github.io   │
│ yêu  │    │ deploy.yml│    │ • Checkout Code      │    │ actions-gh-pages │    │ Xem tab Actions  │
│ cầu  │    │ git push  │    │ • Validate HTML      │    │ → nhánh gh-pages │    │ ✅ / ❌          │
└──────┘    └───────────┘    └──────────────────────┘    └──────────────────┘    └──────────────────┘
     ↑                                                                                     │
     └──────────────────────── sửa code, push lại → pipeline tự chạy lại ─────────────────┘
```

## 4.3 Trả lời câu hỏi

1. **DevOps workflow tự động hóa những bước nào mà cách thủ công phải làm bằng tay?**
   Lấy code (checkout), kiểm tra file (validate HTML), đưa file lên nơi chạy (deploy lên nhánh `gh-pages`) và báo kết quả. Cách thủ công phải làm từng bước này bằng tay.

2. **Nếu deploy sai (website bị lỗi), làm sao quay lại phiên bản cũ?**
   Chạy `git revert <commit>` rồi `git push origin main`. Pipeline tự chạy lại và deploy bản cũ. Có thể dùng `git reset --hard <commit>` + `git push --force`, nhưng cách này ghi đè lịch sử nên chỉ dùng khi làm một mình.

3. **GitHub Actions pipeline tiết kiệm bao nhiêu thời gian so với cách thủ công?**
   Thủ công: chuẩn bị file → upload lên server → cấu hình → kiểm tra, khoảng 15–30 phút mỗi lần. DevOps: một lệnh `git push`, pipeline chạy khoảng 10–20 giây. Mỗi lần deploy tiết kiệm gần như toàn bộ thời gian đó, và không phải làm lại bằng tay ở các lần sau.
