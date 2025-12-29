---

hidemeta: true
---

<style>
/* --- CONTAINER CHÍNH (Chia 2 cột) --- */
.about-container {
display: flex;
align-items: center; /* Căn giữa theo chiều dọc */
gap: 50px; /* Khoảng cách giữa ảnh và chữ */
max-width: 1200px;
margin: 40px auto;
font-family: 'Segoe UI', sans-serif;
}

/* --- CỘT TRÁI: ẢNH --- */
.about-left-col {
flex: 0 0 400px; /* Cố định chiều rộng ảnh khoảng 400px */
position: relative; /* Để đặt cái badge đè lên */
}

.profile-img {
width: 100%;
height: auto;
border-radius: 12px;
box-shadow: 0 0 30px rgba(0, 240, 255, 0.15); /* Bóng neon xanh */
border: 1px solid rgba(0, 240, 255, 0.3);
object-fit: cover;
display: block;
transition: transform 0.3s ease;
}

.profile-img:hover {
transform: scale(1.02);
border-color: #00f0ff;
}

/* BADGE: FUTURE NETWORK ADMIN */
.role-badge {
position: absolute;
bottom: 30px;
left: -20px; /* Cho nó lòi ra bên trái một chút cho nghệ thuật */
background: linear-gradient(90deg, #00c6ff, #0072ff);
padding: 15px 25px;
color: white;
font-weight: bold;
box-shadow: 0 5px 15px rgba(0,0,0,0.3);
border-radius: 4px;
z-index: 10;
}
.role-title {
display: block; font-size: 1.1rem; text-transform: uppercase; letter-spacing: 1px;
}
.role-sub {
font-size: 0.8rem; opacity: 0.9; font-weight: normal;
}

/* --- CỘT PHẢI: THÔNG TIN --- */
.about-right-col {
flex: 1; /* Chiếm hết phần còn lại */
color: #e0e0e0;
font-size: 1.1rem;
line-height: 1.8;
}

/* Highlight text */
.highlight {
color: #00f0ff; font-weight: bold; text-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
}
.target-highlight {
color: #00f0ff; font-weight: bold; border-bottom: 2px dashed #00f0ff;
}

/* Trích dẫn */
.quote-box {
border-left: 4px solid #bc13fe; /* Viền tím */
padding-left: 20px;
margin: 25px 0;
font-style: italic;
color: #ccc;
background: linear-gradient(90deg, rgba(188, 19, 254, 0.1), transparent);
padding: 15px 20px;
border-radius: 0 8px 8px 0;
}

.signature-block {
margin-top: 30px;
text-align: right;
border-top: 1px solid #333;
padding-top: 15px;
}
.sig-name { font-weight: bold; color: #fff; text-transform: uppercase; letter-spacing: 2px; }
.sig-role { font-size: 0.9rem; color: #888; }

/* --- RESPONSIVE (Điện thoại) --- */
@media (max-width: 900px) {
.about-container { flex-direction: column; gap: 30px; }
.about-left-col { flex: auto; width: 100%; max-width: 500px; }
.role-badge { left: 0; bottom: 0; width: 100%; text-align: center; box-sizing: border-box; }
.signature-block { text-align: left; }
}
</style>

<div class="about-container">

<div class="about-left-col">
<img src="/avata100.jpg" class="profile-img" alt="Minh Khanh Portrait">

<div class="role-badge">
<span class="role-title">FUTURE NETWORK ADMIN</span>
<span class="role-sub">HUTECH UNIVERSITY</span>
</div>
</div>

<div class="about-right-col">
<p>
"Là sinh viên năm 4 chuyên ngành Mạng máy tính tại HUTECH, tôi không chỉ học lý thuyết, tôi <span class="highlight">xây dựng hệ thống</span>. Tư duy của tôi là: Làm chủ công nghệ để tối ưu hóa vận hành."
</p>

<div class="quote-box">
"Mục tiêu của tôi là trở thành Network Administrator chuyên nghiệp tại <span class="target-highlight">FPT Telecom</span>. Tôi đam mê hạ tầng Cloud (AWS) và bảo mật hệ thống."
</div>

<p>
Tôi luôn tìm kiếm các giải pháp để kết nối mọi thứ an toàn và hiệu quả hơn. Với tôi, mỗi dòng lệnh cấu hình đều mang một trách nhiệm lớn lao: <b>Giữ cho hệ thống luôn Online.</b>
</p>

<div class="signature-block">
<div class="sig-name">Minh Khanh</div>
<div class="sig-role">Sinh viên HUTECH - K22</div>
</div>
</div>

</div>