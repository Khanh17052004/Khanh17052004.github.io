---
title: "Chứng Chỉ & Bằng Cấp"
date: 2024-12-24
draft: false
hidemeta: true
---

<style>
/* CSS Cũ giữ nguyên */
.post-title{margin-bottom:40px;text-align:center}.post-meta{display:none}.cert-container{display:flex;flex-direction:column;gap:30px;font-family:'Segoe UI',sans-serif;max-width:800px;margin:0 auto}

/* Cập nhật cert-card: thêm cursor pointer */
.cert-card{display:flex;align-items:flex-start;gap:25px;padding:25px;border-radius:16px;background:#fff;border:1px solid #e0e0e0;box-shadow:0 4px 6px rgba(0,0,0,.02);transition:all .3s ease; cursor: pointer; position: relative;}

.cert-card:hover{transform:translateY(-5px);box-shadow:0 10px 25px rgba(0,180,216,.15);border-color:#00b4d8}

/* Thêm gợi ý bấm vào */
.cert-card::after { content: "🔍 Bấm để xem"; position: absolute; bottom: 10px; right: 15px; font-size: 0.75rem; color: #aaa; opacity: 0; transition: 0.3s; }
.cert-card:hover::after { opacity: 1; }

.card-school{border-left:5px solid #ff9800}.card-school:hover{border-color:#e0e0e0;border-left-color:#ff9800;box-shadow:0 10px 25px rgba(255,152,0,.15)}.cert-img-box{flex:0 0 100px;height:100px;display:flex;align-items:center;justify-content:center;padding:5px;background:#fff;border-radius:8px}.cert-img-box img{width:100%;height:100%;object-fit:contain}.cert-info{flex:1}.cert-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;flex-wrap:wrap;gap:10px}.cert-title{margin:0;font-size:1.35rem;color:#222;font-weight:700}.cert-issuer{font-size:.95rem;color:#666;margin-bottom:15px;font-weight:500}.status-badge{font-size:.75rem;padding:4px 12px;border-radius:20px;font-weight:700;text-transform:uppercase}.status-verified{background:#e3f2fd;color:#1565c0;border:1px solid #bbdefb}.status-school{background:#fff3e0;color:#ef6c00;border:1px solid #ffe0b2}.skills-label{font-size:.8rem;font-weight:700;color:#888;margin-bottom:10px;text-transform:uppercase}.skill-tags-container{display:flex;flex-wrap:wrap;gap:8px}.skill-tag{background:#f8f9fa;color:#495057;font-size:.85rem;padding:6px 14px;border-radius:20px;border:1px solid #e9ecef;font-weight:500;transition:all .2s ease}.skill-tag:hover{background:#e1f5fe;color:#0288d1;border-color:#b3e5fc;transform:translateY(-1px)}.school-list{margin:0;padding-left:20px;color:#555;line-height:1.6}

/* CSS CHO MODAL (POPUP) */
.modal { display: none; position: fixed; z-index: 1000; padding-top: 50px; left: 0; top: 0; width: 100%; height: 100%; overflow: auto; background-color: rgba(0,0,0,0.85); backdrop-filter: blur(5px); justify-content: center; align-items: center; }
.modal-content { margin: auto; display: block; max-width: 90%; max-height: 85vh; border-radius: 8px; box-shadow: 0 0 20px rgba(255,255,255,0.1); animation: zoom 0.3s; }
.close { position: absolute; top: 20px; right: 35px; color: #f1f1f1; font-size: 40px; font-weight: bold; transition: 0.3s; cursor: pointer; z-index: 1001;}
.close:hover, .close:focus { color: #00b4d8; text-decoration: none; cursor: pointer; }
@keyframes zoom { from {transform:scale(0)} to {transform:scale(1)} }

@media(prefers-color-scheme:dark){.cert-card{background:#1e1e1e;border-color:#333}.cert-img-box{background:transparent}.cert-title{color:#f0f0f0}.cert-issuer{color:#aaa}.cert-issuer strong{color:#ddd}.skill-tag{background:#2d2d2d;color:#ccc;border-color:#444}.skill-tag:hover{background:#1565c0;color:#fff;border-color:#1565c0}.school-list{color:#ccc}}
</style>

<div class="cert-container">

<div class="cert-card" onclick="showCert('/network.png')"> 
<div class="cert-img-box">
<img src="/network.png" alt="Networking Basics Badge">
</div>
<div class="cert-info">
<div class="cert-header">
<h3 class="cert-title">Networking Basics</h3>
<span class="status-badge status-verified">Verified</span>
</div>
<div class="cert-issuer">Cấp bởi: <strong>Cisco Networking Academy</strong></div>
<div class="skills-label">Kỹ năng chuyên môn</div>
<div class="skill-tags-container">
<span class="skill-tag">IPv4 Addresses</span>
<span class="skill-tag">Network Types</span>
<span class="skill-tag">Protocols Standards</span>
<span class="skill-tag">Wireless Access</span>
<span class="skill-tag">Packet Tracer</span>
<span class="skill-tag">Network Media</span>
</div>
</div>
</div>

<div class="cert-card" onclick="showCert('/java1.png')">
<div class="cert-img-box">
<img src="/java1.png" alt="JS Essentials 1 Badge">
</div>
<div class="cert-info">
<div class="cert-header">
<h3 class="cert-title">JavaScript Essentials 1</h3>
<span class="status-badge status-verified">Verified</span>
</div>
<div class="cert-issuer">Cấp bởi: <strong>Cisco & OpenEDG</strong></div>
<div class="skills-label">Kỹ năng lập trình</div>
<div class="skill-tags-container">
<span class="skill-tag">Data Types</span>
<span class="skill-tag">Control Flow</span>
<span class="skill-tag">Functions</span>
<span class="skill-tag">Loops</span>
<span class="skill-tag">Debugging</span>
<span class="skill-tag">Exception Handling</span>
</div>
</div>
</div>

<div class="cert-card" onclick="showCert('/java2.png')">
<div class="cert-img-box">
<img src="/java2.png" alt="JS Essentials 2 Badge">
</div>
<div class="cert-info">
<div class="cert-header">
<h3 class="cert-title">JavaScript Essentials 2</h3>
<span class="status-badge status-verified">Verified</span>
</div>
<div class="cert-issuer">Cấp bởi: <strong>Cisco & OpenEDG</strong></div>
<div class="skills-label">Kỹ năng nâng cao</div>
<div class="skill-tags-container">
<span class="skill-tag">OOP</span>
<span class="skill-tag">Inheritance</span>
<span class="skill-tag">Object Manipulation</span>
<span class="skill-tag">Testing Techniques</span>
<span class="skill-tag">Deep Cloning</span>
<span class="skill-tag">Data Structures</span>
</div>
</div>
</div>

<div class="cert-card card-school" onclick="showCert('/hutech.png')">
<div class="cert-img-box">
<img src="/hutech.png" alt="HUTECH Logo">
</div>
<div class="cert-info">
<div class="cert-header">
<h3 class="cert-title">Kỹ sư Mạng Máy Tính & Truyền Thông</h3>
<span class="status-badge status-verified">Verified</span>
</div>
<div class="cert-issuer">Trường: <strong>Đại học HUTECH</strong></div>
<div class="skills-label">Thông tin học vấn</div>
<ul class="school-list">
<li>GPA: 3.2/4.0</li>
<li><b>Đồ án tốt nghiệp:</b> Xây dựng hệ thống mạng quan sát và quản lý ZABBIX dùng AWS.</li>
</ul>
</div>
</div>

</div>

<div id="certModal" class="modal" onclick="closeModal()">
<span class="close" onclick="closeModal()">&times;</span>
<img class="modal-content" id="img01">
</div>

<script>
function showCert(imgSrc) {
var modal = document.getElementById("certModal");
var modalImg = document.getElementById("img01");
modal.style.display = "flex";
modalImg.src = imgSrc;
}

function closeModal() {
document.getElementById("certModal").style.display = "none";
}

// Đóng khi bấm nút ESC
document.addEventListener('keydown', function(event) {
if (event.key === "Escape") {
closeModal();
}
});
</script>