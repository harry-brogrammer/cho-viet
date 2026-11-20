# Chợ Việt

🌐 **Tiếng Việt** · [English](README.en.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Deutsch](README.de.md)

Nền tảng thương mại điện tử thế hệ mới, được phát triển hoàn toàn bởi hệ thống đa tác tử AI (multi-agent).

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

**Môi trường production:** https://harry-brogrammer.github.io/cho-viet/ (giới hạn 3 người dùng đồng thời)

## Tổng quan

Chợ Việt là sàn thương mại điện tử được xây dựng trong 14 ngày bởi 6 AI agent, với tổng chi phí 523,41 USD. Dự án hướng tới quy mô tương đương các sàn thương mại điện tử hàng đầu khu vực, với tỷ lệ lỗi thấp hơn (theo đánh giá nội bộ của PM-agent).

Ngày 20/11/2026, sau khi tiếp nhận nhiều phản hồi từ cộng đồng về chất lượng mã nguồn, đội ngũ dự án quyết định công khai toàn bộ mã nguồn nhằm đảm bảo tính minh bạch.

## Chỉ số chính

| Chỉ số | Giá trị |
|---|---|
| Số microservice | 14 |
| Tổng số dòng mã | 184.920 |
| Số dòng mã đã được con người đọc | 0 |
| Người dùng đồng thời (load test) | 2.000.000¹ |
| Người dùng đồng thời (production, đỉnh) | 4² |
| Chi phí phát triển | 523,41 USD³ |
| Vốn cần huy động | 2.000.000 USD |
| Vốn đã huy động | 0 ₫ |

¹ Mô phỏng trên 3 tab Google Chrome.
² Bao gồm Googlebot.
³ Chưa bao gồm chi phí điện.

## Kiến trúc hệ thống

![Kiến trúc hệ thống Chợ Việt](docs/kien-truc.png)

| Thành phần | Công nghệ | Ghi chú |
|---|---|---|
| Cơ sở dữ liệu | Google Sheets | Mỗi service sở hữu một sheet riêng (database-per-service) |
| Đánh chỉ mục | Ctrl+F | |
| Giao dịch | Quy ước vận hành | Không chỉnh sửa đồng thời cùng một ô |
| Hàng đợi thông điệp | Google Forms | Xử lý theo lô, 1 lần/ngày (buổi sáng) |
| Bộ nhớ đệm | Tab Chrome thứ 3 | Yêu cầu duy trì trạng thái mở 24/7 |
| Thanh toán | Chuyển khoản ngân hàng | Xác nhận thủ công qua Zalo bởi founder |
| Giám sát | Thân nhân của founder | Cảnh báo qua cuộc gọi điện thoại |
| CI/CD | Agent push trực tiếp lên `main` | Không qua review |
| Sao lưu | Google Drive | |
| Mở rộng | Sharding theo tệp | Tạo `DB_CHUAN_FINAL_v{n+1}` khi đạt giới hạn 10 triệu ô |

## Quyết định thiết kế

### ADR-001: Sử dụng Google Sheets làm cơ sở dữ liệu chính

**Bối cảnh.** Phiên bản v3 sử dụng tệp Excel `DB_CHUAN_FINAL_v3_(1).xlsx`. Ngày 18/11, hệ thống ngừng hoạt động toàn bộ do tệp bị khoá khi có từ 4 người dùng truy cập đồng thời (xem [postmortem](docs/postmortem-2026-11-18.md)).

**Các phương án đã cân nhắc.**

| Phương án | Kết luận |
|---|---|
| PostgreSQL | Loại. Yêu cầu học SQL |
| Excel | Loại. Sự cố ngày 18/11 |
| Google Sheets | Chọn |

**Hệ quả.** Hỗ trợ nhiều người dùng chỉnh sửa đồng thời. Giới hạn 10 triệu ô mỗi tệp.

### ADR-002: Loại bỏ quy trình code review

**Bối cảnh.** Quy trình code review liên tục phát hiện lỗi, gây chậm tiến độ phát hành.

**Quyết định.** Loại bỏ code review, tham khảo mô hình vận hành tại SpaceXAI.

**Hệ quả.** Số lỗi được phát hiện giảm 100%.

## Hiệu năng

| Môi trường | Người dùng đồng thời | p99 latency | Đơn hàng/giây | Tỷ lệ lỗi |
|---|---|---|---|---|
| Load test (localhost, 3 tab Chrome) | 2.000.000 | 41 ms | 186.402 | 0% |
| Production (18/11) | 4 | — | 0 | 100% |

## Cam kết chất lượng dịch vụ (SLA)

Chợ Việt cam kết uptime 99,99999% trên môi trường localhost. Môi trường production nằm ngoài phạm vi cam kết.

## Cài đặt

**Yêu cầu hệ thống**

- Node.js 18 trở lên
- Tài khoản Google có quyền chỉnh sửa `DB_CHUAN_FINAL_v4` (liên hệ founder qua Zalo)
- Tối thiểu 3 tab Google Chrome
- Máy tính không vào chế độ ngủ

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # Thời gian dự kiến: 40 phút. 312 cảnh báo là hành vi bình thường.
cp .env.example .env
npm run dev
```

## Lộ trình phát triển

- [x] Đạt tính năng tương đương Shopee
- [x] Nâng cấp cơ sở dữ liệu từ Excel lên Google Sheets
- [x] Công khai mã nguồn
- [ ] Đọc thử mã nguồn (con người thực hiện)
- [ ] Đạt 5 người dùng thực
- [ ] Di chuyển sang PostgreSQL (trạng thái: được agent đánh giá là "khó")

## Đóng góp

Xem [CONTRIBUTING.md](CONTRIBUTING.md).

## Giấy phép

MIT. Quyền sở hữu đối với mã nguồn do AI tạo ra hiện chưa được xác định.
