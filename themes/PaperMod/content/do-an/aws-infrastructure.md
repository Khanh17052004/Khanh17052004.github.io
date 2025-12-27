---
title: "Triển khai Hạ tầng Mạng Doanh nghiệp trên AWS"
summary: "Thiết kế hệ thống High Availability và Auto Scaling trên nền tảng Cloud."
weight: 1
---

<style>
/* 1. HERO BANNER */
.project-hero {
    background: linear-gradient(135deg, #232F3E 0%, #3a4b61 100%);
    color: white;
    padding: 40px;
    border-radius: 16px;
    margin-bottom: 40px;
    position: relative;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    border-bottom: 5px solid #FF9900;
}
.hero-title { font-size: 2rem; font-weight: 800; margin: 0 0 10px 0; color: #fff; }
.hero-desc { font-size: 1.1rem; opacity: 0.9; max-width: 800px; }

/* 2. KHUNG MÔ HÌNH */
.model-section {
    text-align: center;
    margin-bottom: 40px;
    background: rgba(255, 255, 255, 0.03);
    padding: 30px;
    border-radius: 16px;
    border: 2px dashed rgba(128, 128, 128, 0.3);
}

.model-img {
    width: 100%;
    max-width: 850px;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
    transition: transform 0.3s ease;
    background: white;
}

.model-img:hover { transform: scale(1.02); }

.model-caption {
    margin-top: 15px;
    font-size: 0.95rem;
    color: #888;
    font-style: italic;
}

/* 3. GRID CÔNG NGHỆ */
.tech-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 20px;
    margin-bottom: 40px;
}
.tech-card {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    padding: 20px;
    border-radius: 12px;
    transition: transform 0.3s ease, border-color 0.3s ease;
}
.tech-card:hover {
    transform: translateY(-5px);
    border-color: #FF9900;
    background: rgba(255, 153, 0, 0.05);
}
.tech-header { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; font-weight: 700; color: #FF9900; font-size: 1.1rem; }
.tech-desc { font-size: 0.95rem; color: #ccc; line-height: 1.5; margin: 0; }

/* Dark Mode Support */
@media (prefers-color-scheme: dark) {
    .tech-desc { color: #b0b0b0; }
}
</style>

<div class="project-hero">
    <h1 class="hero-title">AWS Enterprise Infrastructure</h1>
    <div class="hero-desc">
        Xây dựng hệ thống mạng doanh nghiệp giả lập đảm bảo tính sẵn sàng cao (HA) và khả năng chịu lỗi (Fault Tolerance) chuẩn kiến trúc Well-Architected.
    </div>
</div>

<h2 style="border-left: 4px solid #FF9900; padding-left: 10px; margin-bottom: 20px;">
    📐 Mô hình Kiến trúc (Topology)
</h2>

<div class="model-section">
    <img src="/mohinh.jpg" alt="Sơ đồ kiến trúc mạng AWS" class="model-img">
    <div class="model-caption">
        Hình 1.1: Sơ đồ luồng mạng AWS Infrastructure
    </div>
</div>

<h2 style="border-left: 4px solid #FF9900; padding-left: 10px; margin-bottom: 20px;">
    🛠 Công nghệ & Giải pháp
</h2>

<div class="tech-grid">
    <div class="tech-card">
        <div class="tech-header"><span>🌐</span> VPC & Network</div>
        <p class="tech-desc">Thiết kế kiến trúc <b>Public/Private Subnet</b> tách biệt. Cấu hình NAT Gateway cho Private Subnet và Route Tables tối ưu đường đi.</p>
    </div>
    <div class="tech-card">
        <div class="tech-header"><span>💻</span> EC2 & Auto Scaling</div>
        <p class="tech-desc">Triển khai cụm Server tự động tăng giảm (Scale In/Out) dựa trên CPU Load (>70%), đảm bảo hiệu năng trong giờ cao điểm.</p>
    </div>
    <div class="tech-card">
        <div class="tech-header"><span>⚖️</span> Load Balancer (ALB)</div>
        <p class="tech-desc">Application Load Balancer phân tải traffic thông minh đến các Availability Zones (AZs) khác nhau để tránh nghẽn mạng.</p>
    </div>
    <div class="tech-card">
        <div class="tech-header"><span>🛡</span> Security Group</div>
        <p class="tech-desc">Cấu hình Firewall mềm nhiều lớp (Layered Security). Giới hạn Port nghiêm ngặt (chỉ mở 80/443 cho Web, 22 cho Bastion Host).</p>
    </div>
</div>

<h2 style="border-left: 4px solid #FF9900; padding-left: 10px; margin-bottom: 20px;">
    🎥 Video Demo Hệ Thống
</h2>

<p style="margin-bottom: 20px; color: #ccc;">
    Video dưới đây ghi lại quá trình vận hành thực tế của hệ thống, bao gồm kịch bản Auto Scaling tự động và khả năng chịu lỗi khi giả lập sự cố.
</p>

{{< video src="/videos/demo_aws1.mp4" >}}