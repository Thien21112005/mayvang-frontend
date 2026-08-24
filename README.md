# May Vang Hotel - Frontend Web Application

> Enterprise-grade, responsive, and performance-optimized web client for the May Vang Hotel Management and Reservation Platform.

---

## Language Navigation / Chuyen doi Ngon ngu

- [English Documentation](#english-documentation)
- [Tai lieu Tieng Viet](#tai-lieu-tieng-viet)

---

<a name="english-documentation"></a>
# English Documentation

## 1. Project Overview

The **May Vang Hotel Frontend Client** is a responsive, modern web application engineered to support high-throughput hotel operations and deliver a seamless guest reservation experience. Built strictly on standard web technologies (HTML5 Semantic Markup, CSS3 Modular Styling, and Vanilla JavaScript ES6+), the system eliminates the runtime overhead and bundling complexities of heavy single-page application (SPA) frameworks while preserving high modularity, performance, and clean architectural separation.

### System Objectives
- **Guest and Customer Portal**: Provides real-time room catalog browsing, availability filtering across complex date ranges, automatic membership tier discount computations, multi-step booking pipelines, secure VNPay payment gateway transactions, self-service account management, and verified customer reviews.
- **Administrative & Operations Center**: Delivers real-time business intelligence and KPI dashboards, multi-period revenue tracking, an interactive calendar-based room status matrix, maintenance scheduling, booking order lifecycle tracking, customer lifetime value analytics, manager account provisioning, and customer review moderation.

---

## 2. Architecture & Design Principles

```
+---------------------------------------------------------------------------------+
|                                Client Web Browser                               |
|                                                                                 |
|  +------------------------+  +------------------------+  +-------------------+  |
|  |     HTML5 Document     |  |   CSS3 Modular Styles  |  |   ES6 JavaScript  |  |
|  |  (Semantic Templates)  |  | (Layout, Components)   |  | (Services, Logic) |  |
|  +-----------+------------+  +-----------+------------+  +---------+---------+  |
+--------------|---------------------------|-------------------------|------------+
               |                           |                         |
               v                           v                         v
+---------------------------------------------------------------------------------+
|                         Modular Frontend Script Layers                          |
|                                                                                 |
|  +-----------------------+  +------------------------+  +--------------------+  |
|  |     Presentation      |  |     State & Storage    |  |     API Services   |  |
|  |  - Dynamic Rendering  |  |  - JWT Bearer Tokens   |  |  - Fetch API Engine|  |
|  |  - Form Validation    |  |  - Role / User Profile |  |  - Auth & Booking  |  |
|  |  - Modal Management   |  |  - Toast Notification  |  |  - Admin & Upload  |  |
|  +-----------------------+  +------------------------+  +--------------------+  |
+---------------------------------------------------------------------------------+
                                       |
                   Encrypted HTTPS RESTful Communication (JSON)
                                       |
                                       v
+---------------------------------------------------------------------------------+
|                            Backend Application Server                           |
|                      (Spring Boot 3.3.4 / Java 21 / MySQL)                      |
+---------------------------------------------------------------------------------+
```

### Key Technical Pillars
- **Zero-Dependency Architecture**: Relies on browser-native ES6+ capabilities (Async/Await, Fetch API, FormData, Promises, Web Storage API), eliminating dependency vulnerabilities and complex build toolchains.
- **Separation of Concerns (SoC)**: Script files are decoupled by domain responsibility (Authentication, Room Reservation, Account Management, Operations Management).
- **Session & Role Persistence**: Client session states and JWT claims are managed in browser storage with automated header injection (`Authorization: Bearer <token>`) for protected routes.
- **Third-Party Integrations**:
  - **VNPay Payment Gateway**: Direct URL redirection and cryptographic signature callback parsing for real-time transaction reconciliation.
  - **Cloudinary CDN**: Multipart form data streaming for dynamic avatar and review photo uploads.
  - **Google Identity Services (OAuth 2.0)**: Native Google One-Tap and button sign-in flow.

---

## 3. Directory & File Structure

```
client/
├── asset/                          # Static graphics, iconography, and image assets
│   ├── Background_1.png            # Hero section backdrop artwork
│   ├── Clound01.png - Clound04.png # Layered cloud assets for parallax animations
│   ├── Hotel.png                   # Brand emblem and vector identity
│   ├── Hotel_BG.jpg                # High-definition scenery background
│   ├── Register.png                # Account registration visual asset
│   ├── default-avatar.png          # Fallback placeholder for user profiles
│   └── left_top.png, etc.          # Positional decorative UI ornaments
│
├── css/                            # Modular stylesheets categorized by domain
│   ├── Global.css                  # Design tokens, CSS variables, typography reset
│   ├── Background.css              # Ambient background animations & transitions
│   ├── Header.css                  # Responsive top navigation & dropdown styles
│   ├── Index.css                   # Homepage visual grid, hero search & testimonials
│   ├── room.css                    # Room catalog filters, cards, and booking modal
│   ├── Login.css                   # Authentication card & social sign-in styling
│   ├── Register.css                # Multi-step signup forms & OTP timer styles
│   ├── ResetPW.css                 # 3-phase password recovery layouts
│   ├── profile.css                 # User portal tabs, history table, review modal
│   ├── admin.css                   # Executive KPI cards, room matrix, data tables
│   ├── payment-result.css          # VNPay transaction receipt & status banners
│   ├── Contact.css                 # Contact details, map container, inquiry form
│   ├── CustomSelect.css            # Accessible custom select dropdown component
│   └── CheckStrongPW.css           # Real-time password complexity meter
│
├── js/                             # Modular JavaScript controllers and API clients
│   ├── Header.js                   # Navigation controller & auth session listener
│   ├── SharedHeader.js             # Reusable header component for standalone pages
│   ├── Index.js                    # Landing page room fetcher & review carousel
│   ├── RoomSearch.js               # Availability search engine & booking orchestrator
│   ├── Login.js                    # Credential authentication controller
│   ├── GoogleAuth.js               # Google Identity Services SDK integration
│   ├── Register.js                 # Multi-step customer registration controller
│   ├── OTPHandler.js               # Email OTP delivery, countdown & verification
│   ├── CheckPW.js                  # Client-side password complexity evaluator
│   ├── ResetPW.js                  # Password reset lifecycle manager
│   ├── Profile.js                  # Customer profile, loyalty, bookings & reviews
│   ├── admin.js                    # Admin panel: analytics, rooms, bookings, VIPs
│   ├── payment-result.js           # VNPay return parameter parser & receipt renderer
│   ├── notifications.js            # Unified toast notification engine
│   ├── Contact.js                  # Inquiry form validation and dispatch
│   └── CustomSelect.js             # Accessible custom dropdown event handlers
│
├── index.html                      # Landing page with hero search and reviews
├── room.html                       # Room discovery, filtering, and reservation modal
├── login.html                      # Customer and manager authentication portal
├── register.html                   # Multi-step account registration with email OTP
├── resetpw.html                    # Password recovery and reset portal
├── profile.html                    # Customer portal (profile, tier, orders, reviews)
├── admin.html                      # Manager dashboard and operations console
├── payment-result.html             # VNPay transaction receipt and feedback page
├── contact.html                    # Hotel information, location, and contact form
└── package.json                    # Project configuration and local serve scripts
```

---

## 4. Comprehensive Functional Modules

### 4.1 Room Discovery & Reservation Workflow (`room.html`, `RoomSearch.js`)
- **Dynamic Search Engine**: Validates date intervals (`checkinDate` < `checkoutDate`), number of guests, required room count, and room type categories before querying `/api/rooms/search`.
- **Live Inventory Cards**: Displays high-resolution imagery, base rates, extra amenity surcharges, and bed configurations (e.g., King, Queen, Twin, Sofa bed).
- **Automated Tier Discounting**: Detects authenticated customer status and applies membership discounts dynamically (Silver: 5%, Gold: 10%, Platinum: 15%).
- **Reservation Modal & VNPay Initiation**: Calculates total duration in nights, grand total, creates booking record via `POST /api/bookings`, and redirects instantly to the generated VNPay gateway URL (`POST /api/payments/vnpay/{bookingId}`).

### 4.2 Security & Authentication Lifecycle
- **Standard Authentication (`Login.js`)**: Submits credentials to `POST /api/auth/login`, validates role claims (`CUSTOMER` or `MANAGER`), and stores session tokens.
- **Google OAuth 2.0 (`GoogleAuth.js`)**: Leverages Google Identity Services to capture ID credentials, transmitting them to `POST /api/auth/google` for verification and seamless user onboarding.
- **Two-Step Registration with OTP (`Register.js`, `OTPHandler.js`)**: Collects preliminary account info, requests an email OTP via `POST /api/auth/register`, activates a 60-second client timer, and confirms via `POST /api/auth/register/verify`.
- **Password Strength Evaluation (`CheckPW.js`)**: Enforces complexity standards (minimum 8 characters, uppercase, lowercase, numeric digits, and special characters) in real time.
- **Password Recovery Pipeline (`ResetPW.js`)**: Implements a secure 3-step recovery flow (Request OTP -> Verify Token -> Set New Password).

### 4.3 Customer Self-Service Portal (`profile.html`, `Profile.js`)
- **Personal Profile Management**: Displays and updates user metadata (Full name, phone number, date of birth) via `PUT /api/users/me`.
- **Cloudinary Avatar Upload**: Sends multipart image payloads directly to `POST /api/users/me/avatar`, dynamically updating header and profile avatars.
- **Loyalty & Membership Progression**: Visual progress gauge tracking current point tally against tier thresholds (Bronze: 0, Silver: 800, Gold: 2,500, Platinum: 6,000 points) and displaying tier benefits.
- **Reservation History & Order Lifecycle**: Lists all customer bookings with status indicators (`pending`, `confirmed`, `checked_in`, `checked_out`, `cancelled`). Enables client-side cancellation for pending bookings and payment retry.
- **Customer Reviews & Photo Attachments**: Submits star ratings (1 to 5) and feedback for checked-out reservations, supporting multi-image upload via `POST /api/reviews/me/images`.

### 4.4 Managerial Operations Dashboard (`admin.html`, `admin.js`)
- **Executive Revenue Intelligence**: Fetches financial metrics via `GET /api/manager/revenue?period={week|month|year|all}`, rendering total revenue, completed booking count, and room occupancy rates.
- **Calendar-Based Room Matrix**: Visualizes real-time room occupancy states (`available`, `occupied`, `maintenance`, `inactive`) for any selected date (`GET /api/rooms?date=YYYY-MM-DD`). Allows updating room metadata and scheduling maintenance windows (`PUT /api/rooms/{id}`).
- **Reservation Order Operations**: Real-time multi-filter booking management table (`GET /api/manager/bookings`), filterable by status (`pending`, `confirmed`, `checked_in`, `checked_out`, `cancelled`) and search keyword (Customer name, Booking ID, Room number).
- **VIP Customer Analytics**: Identifies high-value clients via `GET /api/manager/potential-customers`, detailing lifetime spending and booking counts.
- **Staff Provisioning**: Enables existing managers to provision new manager accounts via `POST /api/manager/managers`.
- **Review Moderation & Responses**: Allows managers to inspect customer reviews and post official responses (`PUT /api/reviews/{id}/reply`).

### 4.5 Payment Verification & Receipt Rendering (`payment-result.html`, `payment-result.js`)
- Captures and decodes URL parameters returned by the backend payment return handler (`status`, `message`, `bookingId`, `amount`, `transactionCode`, `earnedPoint`, `bookingStatus`).
- Renders official transaction receipts, indicates points credited to the user's loyalty account, and provides navigation back to user orders.

---

## 5. API Integration Mapping

| Domain | Frontend Script | Backend Endpoint | Method | Security Scope |
|---|---|---|---|---|
| **Auth** | `Login.js` | `/api/auth/login` | POST | Public |
| **Auth** | `Register.js` | `/api/auth/register` | POST | Public |
| **Auth** | `Register.js` | `/api/auth/register/verify` | POST | Public |
| **Auth** | `GoogleAuth.js` | `/api/auth/google` | POST | Public |
| **Auth** | `ResetPW.js` | `/api/auth/forgot-password` | POST | Public |
| **Auth** | `ResetPW.js` | `/api/auth/verify-otp` | POST | Public |
| **Auth** | `ResetPW.js` | `/api/auth/reset-password` | POST | Public |
| **Rooms** | `RoomSearch.js` | `/api/rooms/search` | GET | Public |
| **Rooms** | `admin.js` | `/api/rooms` | GET | Public / Manager |
| **Rooms** | `admin.js` | `/api/rooms/{id}` | PUT | `ROLE_MANAGER` |
| **Bookings**| `RoomSearch.js` | `/api/bookings` | POST | `ROLE_CUSTOMER` |
| **Payments**| `RoomSearch.js` | `/api/payments/vnpay/{id}` | POST | `ROLE_CUSTOMER` |
| **Payments**| `payment-result.js` | `/api/payments/vnpay-return` | GET | Public |
| **Profile** | `Profile.js`, `Header.js` | `/api/users/me` | GET / PUT | `ROLE_CUSTOMER` |
| **Profile** | `Profile.js` | `/api/users/me/avatar` | POST | `ROLE_CUSTOMER` |
| **Profile** | `Profile.js` | `/api/users/me/password` | PUT | `ROLE_CUSTOMER` |
| **Profile** | `Profile.js` | `/api/users/me/bookings` | GET | `ROLE_CUSTOMER` |
| **Profile** | `Profile.js` | `/api/users/me/bookings/{id}/cancel` | PUT | `ROLE_CUSTOMER` |
| **Reviews** | `Index.js`, `admin.js` | `/api/reviews/public` | GET | Public |
| **Reviews** | `Profile.js` | `/api/reviews/me` | GET | `ROLE_CUSTOMER` |
| **Reviews** | `Profile.js` | `/api/reviews/` | POST | `ROLE_CUSTOMER` |
| **Reviews** | `Profile.js` | `/api/reviews/me/images` | POST | `ROLE_CUSTOMER` |
| **Reviews** | `admin.js` | `/api/reviews/{id}/reply` | PUT / DELETE | `ROLE_MANAGER` |
| **Manager** | `admin.js` | `/api/manager/revenue` | GET | `ROLE_MANAGER` |
| **Manager** | `admin.js` | `/api/manager/potential-customers` | GET | `ROLE_MANAGER` |
| **Manager** | `admin.js` | `/api/manager/bookings` | GET | `ROLE_MANAGER` |
| **Manager** | `admin.js` | `/api/manager/managers` | POST | `ROLE_MANAGER` |

## 6. Installation & Execution Guide

### 6.1 Prerequisites
- Modern Web Browser (Google Chrome, Mozilla Firefox, Microsoft Edge, Safari).
- Node.js (version 14.0.0 or higher) for local server execution, OR any standard static web server.

### 6.2 Base URL Configuration
By default, the client scripts target the live deployment API:
```javascript
// Production Endpoint
this.baseUrl = "https://mayvang-api.onrender.com/api";
```

To configure for local backend testing, update the target endpoint in `client/js/` files:
```javascript
// Local Development Endpoint
this.baseUrl = "http://localhost:8080/api";
```

### 6.3 Local Development Server

#### Option 1: Using Node Package Manager (NPM)
```bash
# Navigate to the client directory
cd client

# Launch local server
npm start
```

#### Option 2: Using NPX Serve directly
```bash
npx serve . -l 3000
```

#### Option 3: Using Python Built-in HTTP Server
```bash
# Python 3
python -m http.server 3000
```

#### Option 4: Visual Studio Code Live Server
Open the `client` directory in Visual Studio Code, right-click `index.html`, and select **Open with Live Server**.

## 7. Security & Quality Assurance

- **Cross-Site Scripting (XSS) Prevention**: All dynamic HTML injections are sanitized using entity encoding or explicit DOM text node bindings to prevent malicious script injection.
- **Authorization Guarding**: Protected interfaces (e.g., `admin.html`, `profile.html`) verify the presence of valid tokens upon page load and enforce automatic redirection for unauthorized access attempts.
- **State Integrity**: Session parameters are updated upon profile modifications, ensuring that UI indicators accurately reflect user loyalty tiers, point balances, and credentials.

---

<a name="tai-lieu-tieng-viet"></a>
# Tài liệu Tiếng Việt

## 1. Tổng quan Dự án

**Giao diện Web Khách sạn Mây Vàng (Frontend Client)** là hệ thống ứng dụng web chuyên nghiệp, tối ưu hiệu năng và tương thích đa thiết bị, phục vụ đồng thời cho khách hàng có nhu cầu tra cứu, đặt phòng trực tuyến và đội ngũ quản lý vận hành khách sạn.

Dự án được xây dựng hoàn toàn bằng các công nghệ web tiêu chuẩn (HTML5 Semantic, CSS3 Module hóa và JavaScript ES6+ thuần), loại bỏ sự phụ thuộc vào các framework single-page application nặng nề, mang lại tốc độ tải trang tối ưu, khả năng tương thích cao và dễ dàng bảo trì.

### Mục tiêu Chức năng Chính
- **Phân hệ Khách hàng**: Cung cấp công cụ tra cứu phòng theo thời gian thực, lọc danh sách phòng theo tiêu chí phức tạp, tự động tính giá chiết khấu theo hạng thành viên, quy trình đặt phòng đa bước, thanh toán trực tuyến an toàn qua cổng VNPay, quản lý thông tin cá nhân và gửi đánh giá dịch vụ.
- **Phân hệ Quản lý (Admin Operations)**: Cung cấp bảng điều khiển chỉ số kinh doanh thời gian thực, thống kê doanh thu theo nhiều chu kỳ, ma trận trạng thái phòng theo lịch, thiết lập lịch bảo trì phòng, quản lý đơn đặt phòng, phân tích khách hàng tiềm năng (VIP), tạo tài khoản quản lý và phản hồi đánh giá của khách hàng.

## 2. Kiến trúc & Nguyên lý Thiết kế

```
+---------------------------------------------------------------------------------+
|                               Trình duyệt Người dùng                            |
|                                                                                 |
|  +------------------------+  +------------------------+  +-------------------+  |
|  |     Trang HTML5        |  |     Định kiểu CSS3     |  |    JavaScript ES6+|  |
|  |   (Giao diện Chuẩn)    |  |  (Module hóa Giao diện)|  | (Xử lý Nghiệp vụ) |  |
|  +-----------+------------+  +-----------+------------+  +---------+---------+  |
+--------------|---------------------------|-------------------------|------------+
               |                           |                         |
               v                           v                         v
+---------------------------------------------------------------------------------+
|                         Các Tầng Xử lý Phía Client                              |
|                                                                                 |
|  +-----------------------+  +------------------------+  +--------------------+  |
|  |      Giao diện        |  |  Trạng thái & Session  |  |    Kết nối API     |  |
|  |  - Render DOM động    |  |  - Token JWT Bearer    |  |  - Fetch API Engine|  |
|  |  - Kiểm tra biểu mẫu  |  |  - Vai trò & Hồ sơ     |  |  - Auth & Đặt phòng|  |
|  |  - Hộp thoại (Modal)  |  |  - Thông báo Toast     |  |  - Admin & Upload  |  |
|  +-----------------------+  +------------------------+  +--------------------+  |
+---------------------------------------------------------------------------------+
                                       |
                     Giao thức HTTPS RESTful Mã hóa (JSON)
                                       |
                                       v
+---------------------------------------------------------------------------------+
|                           Máy chủ Backend REST API                              |
|                     (Spring Boot 3.3.4 / Java 21 / MySQL)                       |
+---------------------------------------------------------------------------------+
```

### Đặc điểm Kỹ thuật Nổi bật
- **Kiến trúc Không phụ thuộc Thư viện nặng (Zero-Dependency)**: Sử dụng hoàn toàn các API có sẵn của trình duyệt (Async/Await, Fetch API, FormData, Promises, Web Storage), giúp tối thiểu dung lượng tải trang và loại bỏ rủi ro bảo mật từ các gói thư viện bên ngoài.
- **Tách biệt Trách nhiệm (Separation of Concerns)**: Các tệp script được phân chia tách bạch theo từng miền nghiệp vụ (Xác thực, Đặt phòng, Hồ sơ Khách hàng, Quản trị Hệ thống).
- **Quản lý Phiên & Phân quyền**: Lưu trữ JWT và thông tin vai trò tại `localStorage`, tự động đính kèm tiêu đề `Authorization: Bearer <token>` trong mọi yêu cầu gọi API tới các endpoint được bảo vệ.
- **Tích hợp Dịch vụ Ngoại vi**:
  - **Cổng thanh toán VNPay**: Điều hướng thanh toán và giải mã chữ ký số xác thực kết quả giao dịch thời gian thực.
  - **Cloudinary CDN**: Gửi dữ liệu form multipart để tải ảnh đại diện và ảnh đánh giá lên đám mây.
  - **Google Identity Services (OAuth 2.0)**: Tích hợp cơ chế đăng nhập 1 chạm với tài khoản Google.

## 3. Cấu trúc Thư mục & Tệp tin

```
client/
├── asset/                          # Thư mục tài nguyên hình ảnh và đồ họa tĩnh
│   ├── Background_1.png            # Ảnh nền chính khu vực Hero
│   ├── Clound01.png - Clound04.png # Các lớp đồ họa mây tạo hiệu ứng Parallax
│   ├── Hotel.png                   # Logo và biểu tượng khách sạn
│   ├── Hotel_BG.jpg                # Hình nền phong cảnh độ phân giải cao
│   ├── Register.png                # Hình ảnh minh họa trang đăng ký
│   ├── default-avatar.png          # Ảnh đại diện mặc định cho người dùng
│   └── left_top.png, v.v.          # Hoa văn trang trí góc giao diện
│
├── css/                            # Hệ thống stylesheet được phân chia theo trang
│   ├── Global.css                  # Biến màu sắc, font chữ, reset CSS mặc định
│   ├── Background.css              # Định kiểu hiệu ứng mây trôi và hình nền động
│   ├── Header.css                  # Định kiểu thanh điều hướng Header responsive
│   ├── Index.css                   # Layout trang chủ, thẻ phòng, đánh giá khách hàng
│   ├── room.css                    # Bộ lọc tìm kiếm, thẻ phòng và modal đặt phòng
│   ├── Login.css                   # Giao diện form đăng nhập và nút social
│   ├── Register.css                # Giao diện đăng ký 2 bước và bộ đếm OTP
│   ├── ResetPW.css                 # Giao diện 3 bước khôi phục mật khẩu
│   ├── profile.css                 # Giao diện hồ sơ, bảng hạng thẻ, đơn phòng, modal
│   ├── admin.css                   # Giao diện quản lý, bảng thống kê, ma trận phòng
│   ├── payment-result.css          # Giao diện biên nhận giao dịch VNPay
│   ├── Contact.css                 # Giao diện liên hệ và biểu mẫu gửi thông tin
│   ├── CustomSelect.css            # Thành phần dropdown chọn tùy chỉnh
│   └── CheckStrongPW.css           # Thanh đo độ mạnh mật khẩu thời gian thực
│
├── js/                             # Các module JavaScript xử lý logic và API
│   ├── Header.js                   # Điều khiển Header và kiểm tra trạng thái đăng nhập
│   ├── SharedHeader.js             # Hỗ trợ nhúng Header chung cho các trang độc lập
│   ├── Index.js                    # Tải phòng nổi bật và đánh giá khách hàng trang chủ
│   ├── RoomSearch.js               # Logic tìm kiếm phòng, tính giá, mở modal, gọi VNPay
│   ├── Login.js                    # Xử lý đăng nhập bằng tài khoản và lưu phiên
│   ├── GoogleAuth.js               # Tích hợp SDK Google Identity Services
│   ├── Register.js                 # Điều phối luồng đăng ký tài khoản 2 bước
│   ├── OTPHandler.js               # Quản lý gửi mã OTP, đếm ngược 60s và xác minh
│   ├── CheckPW.js                  # Kiểm tra độ an toàn mật khẩu thời gian thực
│   ├── ResetPW.js                  # Xử lý quy trình 3 bước đặt lại mật khẩu qua OTP
│   ├── Profile.js                  # Quản lý thông tin cá nhân, hạng thẻ, đơn phòng, review
│   ├── admin.js                    # Nghiệp vụ admin: doanh thu, ma trận phòng, đơn hàng, VIP
│   ├── payment-result.js           # Bóc tách tham số phản hồi VNPay và render hóa đơn
│   ├── notifications.js            # Hệ thống thông báo Toast đồng bộ
│   ├── Contact.js                  # Kiểm tra và gửi biểu mẫu liên hệ
│   └── CustomSelect.js             # Lắng nghe sự kiện cho dropdown lựa chọn
│
├── index.html                      # Trang chủ giới thiệu và tìm kiếm nhanh
├── room.html                       # Trang danh mục phòng, bộ lọc và đặt phòng
├── login.html                      # Trang đăng nhập hệ thống
├── register.html                   # Trang đăng ký thành viên xác thực OTP email
├── resetpw.html                    # Trang khôi phục mật khẩu
├── profile.html                    # Cổng thông tin cá nhân, tích điểm, lịch sử đơn
├── admin.html                      # Trung tâm quản trị và điều hành vận hành
├── payment-result.html             # Trang hiển thị kết quả giao dịch VNPay
├── contact.html                    # Trang thông tin liên hệ và bản đồ
└── package.json                    # Khai báo thông tin dự án và lệnh khởi chạy
```

## 4. Chi tiết Các Phân hệ Chức năng

### 4.1 Tra cứu & Đặt phòng Trực tuyến (`room.html`, `RoomSearch.js`)
- **Bộ lọc Thông minh**: Kiểm tra tính hợp lệ của khoảng thời gian (`checkinDate` < `checkoutDate`), số lượng khách, số phòng cần thuê và loại phòng trước khi gửi yêu cầu tới `/api/rooms/search`.
- **Hiển thị Danh mục Phòng**: Trình bày hình ảnh chất lượng cao, đơn giá gốc, phụ phí vị trí/hướng nhìn và cấu hình giường (Đơn, Đôi, Queen, King, Sofa bed).
- **Tự động Chiết khấu theo Hạng**: Nhận diện phiên đăng nhập của khách hàng để tự động áp dụng tỉ lệ giảm giá tương ứng (Silver: 5%, Gold: 10%, Platinum: 15%).
- **Hộp thoại Xác nhận & Chuyển hướng VNPay**: Tính toán tổng số đêm lưu trú, tổng chi phí sau chiết khấu, khởi tạo đơn hàng qua `POST /api/bookings` và chuyển hướng trực tiếp tới cổng VNPay (`POST /api/payments/vnpay/{bookingId}`).

### 4.2 Hệ thống Xác thực & Bảo mật Tài khoản
- **Đăng nhập Truyền thống (`Login.js`)**: Gửi thông tin đăng nhập tới `POST /api/auth/login`, kiểm tra vai trò (`CUSTOMER` hoặc `MANAGER`) và lưu trữ token phiên.
- **Đăng nhập Google OAuth 2.0 (`GoogleAuth.js`)**: Thu nhận token ID từ Google Identity Services và gửi xác thực tại `POST /api/auth/google`.
- **Đăng ký 2 Bước với OTP Email (`Register.js`, `OTPHandler.js`)**: Thu thập thông tin cơ bản, gửi mã OTP 6 chữ số qua email tại `POST /api/auth/register`, kích hoạt bộ đếm 60 giây và xác thực tại `POST /api/auth/register/verify`.
- **Kiểm soát Độ mạnh Mật khẩu (`CheckPW.js`)**: Bắt buộc mật khẩu tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, chữ số và ký tự đặc biệt theo thời gian thực.
- **Khôi phục Mật khẩu (`ResetPW.js`)**: Quy trình 3 bước an toàn (Yêu cầu OTP -> Xác minh mã -> Thiết lập mật khẩu mới).

### 4.3 Cổng Thông tin Khách hàng (`profile.html`, `Profile.js`)
- **Thông tin Cá nhân**: Xem và cập nhật họ tên, số điện thoại, ngày sinh qua `PUT /api/users/me`.
- **Tải Ảnh đại diện Cloudinary**: Gửi dữ liệu ảnh multipart tới `POST /api/users/me/avatar`, tự động cập nhật ảnh đại diện trên toàn giao diện.
- **Theo dõi Hạng Thành viên**: Thanh tiến trình trực quan thể hiện điểm tích lũy hiện tại so với các cột mốc hạng thẻ (Bronze: 0, Silver: 800, Gold: 2.500, Platinum: 6.000 điểm) cùng danh sách quyền lợi.
- **Lịch sử Đơn đặt phòng**: Bảng quản lý toàn bộ đơn đặt phòng với nhãn trạng thái rõ ràng (`pending`, `confirmed`, `checked_in`, `checked_out`, `cancelled`). Hỗ trợ khách hàng tự hủy đơn `pending` và thanh toán lại qua VNPay.
- **Đánh giá & Đính kèm Ảnh**: Gửi đánh giá số sao (1 - 5 sao) và ý kiến phản hồi cho các đơn đã hoàn tất, hỗ trợ tải nhiều ảnh thực tế qua `POST /api/reviews/me/images`.

### 4.4 Bảng Điều khiển Quản trị (`admin.html`, `admin.js`)
- **Thống kê Doanh thu**: Thu thập dữ liệu tài chính qua `GET /api/manager/revenue?period={week|month|year|all}`, tính toán tổng doanh thu, số đơn thành công và tỷ lệ lấp đầy phòng.
- **Ma trận Trạng thái Phòng theo Ngày**: Trực quan hóa tình trạng phòng (`available`, `occupied`, `maintenance`, `inactive`) theo ngày được chọn (`GET /api/rooms?date=YYYY-MM-DD`). Cho phép cập nhật thông tin và thiết lập lịch bảo trì phòng (`PUT /api/rooms/{id}`).
- **Xử lý Đơn đặt phòng**: Bảng tra cứu đơn hàng theo nhiều tiêu chí (`GET /api/manager/bookings`), lọc theo trạng thái đơn và tìm kiếm theo tên, mã đơn MV-ID hoặc số điện thoại.
- **Phân tích Khách hàng VIP**: Xếp hạng khách hàng thân thiết dựa trên tổng chi tiêu và tần suất đặt phòng (`GET /api/manager/potential-customers`).
- **Cấp tài khoản Quản lý**: Tạo mới tài khoản quản trị viên an toàn qua `POST /api/manager/managers`.
- **Phản hồi Đánh giá**: Xem xét và gửi phản hồi chính thức tới các đánh giá của khách hàng (`PUT /api/reviews/{id}/reply`).

### 4.5 Xử lý Kết quả Giao dịch VNPay (`payment-result.html`, `payment-result.js`)
- Tiếp nhận và giải mã các tham số phản hồi từ máy chủ sau khi thanh toán (`status`, `message`, `bookingId`, `amount`, `transactionCode`, `earnedPoint`, `bookingStatus`).
- Hiển thị hóa đơn giao dịch chi tiết, thông báo số điểm thưởng vừa tích lũy và cung cấp liên kết trở về lịch sử đơn hàng.

## 5. Bảng Ánh xạ Endpoint API

| Nghiệp vụ | File Script Frontend | Endpoint Backend | Method | Quyền hạn |
|---|---|---|---|---|
| **Xác thực** | `Login.js` | `/api/auth/login` | POST | Công khai |
| **Xác thực** | `Register.js` | `/api/auth/register` | POST | Công khai |
| **Xác thực** | `Register.js` | `/api/auth/register/verify` | POST | Công khai |
| **Xác thực** | `GoogleAuth.js` | `/api/auth/google` | POST | Công khai |
| **Xác thực** | `ResetPW.js` | `/api/auth/forgot-password` | POST | Công khai |
| **Xác thực** | `ResetPW.js` | `/api/auth/verify-otp` | POST | Công khai |
| **Xác thực** | `ResetPW.js` | `/api/auth/reset-password` | POST | Công khai |
| **Tra cứu** | `RoomSearch.js` | `/api/rooms/search` | GET | Công khai |
| **Phòng** | `admin.js` | `/api/rooms` | GET | Công khai / Quản lý |
| **Phòng** | `admin.js` | `/api/rooms/{id}` | PUT | `ROLE_MANAGER` |
| **Đặt phòng**| `RoomSearch.js` | `/api/bookings` | POST | `ROLE_CUSTOMER` |
| **Thanh toán**| `RoomSearch.js` | `/api/payments/vnpay/{id}` | POST | `ROLE_CUSTOMER` |
| **Thanh toán**| `payment-result.js` | `/api/payments/vnpay-return` | GET | Công khai |
| **Hồ sơ** | `Profile.js`, `Header.js` | `/api/users/me` | GET / PUT | `ROLE_CUSTOMER` |
| **Hồ sơ** | `Profile.js` | `/api/users/me/avatar` | POST | `ROLE_CUSTOMER` |
| **Hồ sơ** | `Profile.js` | `/api/users/me/password` | PUT | `ROLE_CUSTOMER` |
| **Hồ sơ** | `Profile.js` | `/api/users/me/bookings` | GET | `ROLE_CUSTOMER` |
| **Hồ sơ** | `Profile.js` | `/api/users/me/bookings/{id}/cancel` | PUT | `ROLE_CUSTOMER` |
| **Đánh giá** | `Index.js`, `admin.js` | `/api/reviews/public` | GET | Công khai |
| **Đánh giá** | `Profile.js` | `/api/reviews/me` | GET | `ROLE_CUSTOMER` |
| **Đánh giá** | `Profile.js` | `/api/reviews/` | POST | `ROLE_CUSTOMER` |
| **Đánh giá** | `Profile.js` | `/api/reviews/me/images` | POST | `ROLE_CUSTOMER` |
| **Đánh giá** | `admin.js` | `/api/reviews/{id}/reply` | PUT / DELETE | `ROLE_MANAGER` |
| **Quản trị** | `admin.js` | `/api/manager/revenue` | GET | `ROLE_MANAGER` |
| **Quản trị** | `admin.js` | `/api/manager/potential-customers` | GET | `ROLE_MANAGER` |
| **Quản trị** | `admin.js` | `/api/manager/bookings` | GET | `ROLE_MANAGER` |
| **Quản trị** | `admin.js` | `/api/manager/managers` | POST | `ROLE_MANAGER` |

## 6. Hướng dẫn Cài đặt & Khởi chạy

### 6.1 Yêu cầu Hệ thống
- Trình duyệt web hiện đại (Google Chrome, Mozilla Firefox, Microsoft Edge, Safari).
- Node.js (phiên bản >= 14.0.0) để khởi chạy local server, HOẶC bất kỳ máy chủ web tĩnh nào.

### 6.2 Cấu hình Địa chỉ API Backend
Mặc định client kết nối tới máy chủ Production:
```javascript
// Endpoint Production
this.baseUrl = "https://mayvang-api.onrender.com/api";
```

Khi chạy kiểm thử với Backend trên máy cục bộ, thay đổi giá trị trong các tệp `client/js/`:
```javascript
// Endpoint Localhost
this.baseUrl = "http://localhost:8080/api";
```

### 6.3 Khởi chạy Server Cục bộ

#### Cách 1: Sử dụng NPM
```bash
# Di chuyển vào thư mục client
cd client

# Khởi chạy server
npm start
```

#### Cách 2: Sử dụng NPX Serve trực tiếp
```bash
npx serve . -l 3000
```

#### Cách 3: Sử dụng Python HTTP Server
```bash
# Python 3
python -m http.server 3000
```

#### Cách 4: Sử dụng Live Server trên Visual Studio Code
Mở thư mục `client` trong VS Code, nhấp chuột phải vào file `index.html` và chọn **Open with Live Server**.

## 7. Tiêu chuẩn Bảo mật & Chất lượng

- **Phòng chống XSS**: Mọi dữ liệu từ máy chủ được mã hóa ký tự đặc biệt trước khi đưa vào DOM hoặc sử dụng phương thức gán text thuần túy.
- **Kiểm soát Quyền truy cập**: Các trang quản trị (`admin.html`) và cá nhân (`profile.html`) đều có cơ chế kiểm tra token và tự động chuyển hướng nếu người dùng chưa đăng nhập hoặc không đúng vai trò.
- **Đồng bộ Trạng thái**: Các thay đổi về hồ sơ và điểm thưởng được cập nhật ngay lập tức lên giao diện mà không cần tải lại toàn bộ trang.
