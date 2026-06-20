<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>หลังบ้าน — จัดการสินค้า</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Sarabun', sans-serif; background: #f5f5f0; color: #1a1a1a; min-height: 100vh; }

  header { background: #1a1a2e; color: white; padding: 1rem 2rem; display: flex; align-items: center; gap: 12px; }
  header h1 { font-size: 18px; font-weight: 600; }
  header span { font-size: 13px; opacity: 0.6; }

  .container { max-width: 960px; margin: 0 auto; padding: 2rem 1rem; }

  .card { background: white; border-radius: 12px; border: 1px solid #e5e5e0; padding: 1.5rem; margin-bottom: 1.5rem; }
  .card h2 { font-size: 16px; font-weight: 600; margin-bottom: 1.25rem; color: #1a1a2e; display: flex; align-items: center; gap: 8px; }

  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
  @media (max-width: 600px) { .form-grid { grid-template-columns: 1fr; } }
  .form-group { display: flex; flex-direction: column; gap: 6px; }
  .form-group.full { grid-column: 1 / -1; }
  label { font-size: 13px; font-weight: 500; color: #555; }

  input[type=text], input[type=number], textarea, select {
    padding: 10px 12px; border: 1px solid #ddd; border-radius: 8px;
    font-size: 14px; font-family: inherit; color: #1a1a1a;
    outline: none; transition: border-color 0.2s; background: white; width: 100%;
  }
  input:focus, textarea:focus, select:focus { border-color: #534AB7; box-shadow: 0 0 0 3px rgba(83,74,183,0.1); }
  textarea { resize: vertical; min-height: 80px; }

  .image-area {
    border: 2px dashed #d0d0d0; border-radius: 8px; padding: 1.5rem;
    text-align: center; cursor: pointer; position: relative;
    min-height: 140px; display: flex; flex-direction: column;
    align-items: center; justify-content: center; gap: 8px; color: #888; font-size: 13px;
  }
  .image-area:hover { border-color: #534AB7; color: #534AB7; }
  .image-area input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
  .image-area i { font-size: 28px; }

  .btn { padding: 10px 20px; border-radius: 8px; font-size: 14px; font-weight: 500; font-family: inherit; cursor: pointer; border: none; display: inline-flex; align-items: center; gap: 6px; transition: all 0.15s; }
  .btn-primary { background: #534AB7; color: white; }
  .btn-primary:hover { background: #3C3489; }
  .btn-danger { background: #fee2e2; color: #991b1b; }
  .btn-danger:hover { background: #fecaca; }
  .btn-sm { padding: 6px 12px; font-size: 13px; }
  .btn-outline { background: transparent; border: 1px solid #ddd; color: #555; }
  .btn-outline:hover { background: #f5f5f0; }

  .actions { display: flex; gap: 10px; margin-top: 1.25rem; justify-content: flex-end; }

  .product-table { width: 100%; border-collapse: collapse; font-size: 14px; }
  .product-table th { text-align: left; padding: 10px 12px; border-bottom: 2px solid #e5e5e0; font-size: 13px; font-weight: 600; color: #555; background: #f9f9f7; }
  .product-table td { padding: 12px; border-bottom: 1px solid #f0f0ec; vertical-align: middle; }
  .product-table tr:last-child td { border-bottom: none; }
  .product-table tr:hover td { background: #fafaf8; }

  .thumb { width: 50px; height: 50px; object-fit: cover; border-radius: 6px; border: 1px solid #eee; }
  .thumb-placeholder { width: 50px; height: 50px; border-radius: 6px; background: #f0f0ec; display: flex; align-items: center; justify-content: center; color: #aaa; font-size: 20px; }

  .badge { display: inline-block; padding: 3px 10px; border-radius: 20px; font-size: 12px; font-weight: 500; }
  .badge-active { background: #dcfce7; color: #166534; }
  .badge-inactive { background: #f3f4f6; color: #6b7280; }
  .price { font-weight: 600; color: #534AB7; }

  .empty-state { text-align: center; padding: 3rem; color: #aaa; }
  .empty-state i { font-size: 48px; display: block; margin-bottom: 1rem; }

  .toast { position: fixed; bottom: 2rem; right: 2rem; background: #1a1a2e; color: white; padding: 12px 20px; border-radius: 8px; font-size: 14px; display: none; align-items: center; gap: 8px; z-index: 999; box-shadow: 0 4px 16px rgba(0,0,0,0.2); }
  .toast.show { display: flex; }

  .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; margin-bottom: 1.5rem; }
  .stat { background: white; border-radius: 10px; padding: 1rem 1.25rem; border: 1px solid #e5e5e0; }
  .stat .num { font-size: 22px; font-weight: 600; color: #1a1a2e; }
  .stat .lbl { font-size: 12px; color: #888; margin-top: 2px; }

  .line-btn { background: #06C755; color: white; padding: 8px 16px; border-radius: 8px; font-size: 13px; font-weight: 500; border: none; cursor: pointer; display: inline-flex; align-items: center; gap: 6px; font-family: inherit; }
  .line-btn:hover { background: #05a847; }
  .line-config { display: flex; gap: 10px; align-items: center; }
  .line-config input { flex: 1; }

  .tab-bar { display: flex; gap: 4px; margin-bottom: 1.5rem; }
  .tab { padding: 8px 16px; border-radius: 8px; font-size: 14px; font-weight: 500; cursor: pointer; border: 1px solid transparent; color: #555; background: transparent; font-family: inherit; }
  .tab.active { background: #534AB7; color: white; border-color: #534AB7; }
  .tab:hover:not(.active) { background: #f0f0ec; }

  .loading { text-align: center; padding: 2rem; color: #aaa; font-size: 14px; }
  .loading i { font-size: 28px; display: block; margin-bottom: 8px; animation: spin 1s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  .api-note { background: #f0f0ec; border-radius: 8px; padding: 10px 14px; font-size: 12px; color: #888; word-break: break-all; margin-bottom: 1rem; }
</style>
</head>
<body>

<header>
  <i class="ti ti-layout-dashboard" style="font-size:22px"></i>
  <div>
    <h1>หลังบ้าน — จัดการสินค้า</h1>
    <span>เชื่อมต่อ Google Sheets</span>
  </div>
  <div style="margin-left:auto; display:flex; gap:10px;">
    <a href="shop.html" target="_blank" class="btn btn-outline" style="font-size:13px;padding:7px 14px;">
      <i class="ti ti-external-link"></i> ดูหน้าร้าน
    </a>
  </div>
</header>

<div class="container">

  <div class="stats">
    <div class="stat"><div class="num" id="stat-total">—</div><div class="lbl">สินค้าทั้งหมด</div></div>
    <div class="stat"><div class="num" id="stat-active">—</div><div class="lbl">สินค้าที่แสดง</div></div>
    <div class="stat"><div class="num" id="stat-sync">⏳</div><div class="lbl">สถานะ Google Sheets</div></div>
  </div>

  <div class="tab-bar">
    <button class="tab active" onclick="showTab('products')">สินค้า</button>
    <button class="tab" onclick="showTab('add')">เพิ่มสินค้า</button>
    <button class="tab" onclick="showTab('settings')">ตั้งค่า</button>
  </div>

  <!-- TAB: PRODUCTS -->
  <div id="tab-products">
    <div class="card">
      <h2><i class="ti ti-packages"></i> รายการสินค้า
        <button class="btn btn-outline btn-sm" style="margin-left:auto;" onclick="loadProducts()">
          <i class="ti ti-refresh"></i> รีเฟรช
        </button>
      </h2>
      <div id="product-list-wrap">
        <div class="loading" id="loading-msg"><i class="ti ti-loader"></i> กำลังโหลดจาก Google Sheets...</div>
        <div class="empty-state" id="empty-msg" style="display:none;"><i class="ti ti-shopping-bag"></i> ยังไม่มีสินค้า</div>
        <div style="overflow-x:auto;">
          <table class="product-table" id="product-table" style="display:none;">
            <thead>
              <tr>
                <th>รูป</th><th>ชื่อสินค้า</th><th>ราคา</th><th>หมวดหมู่</th><th>สถานะ</th><th>จัดการ</th>
              </tr>
            </thead>
            <tbody id="product-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <!-- TAB: ADD -->
  <div id="tab-add" style="display:none;">
    <div class="card">
      <h2><i class="ti ti-circle-plus"></i> <span id="form-title">เพิ่มสินค้าใหม่</span></h2>
      <input type="hidden" id="edit-id">
      <div class="form-grid">
        <div class="form-group full">
          <label>ชื่อสินค้า *</label>
          <input type="text" id="f-name" placeholder="เช่น เสื้อยืดสีขาว">
        </div>
        <div class="form-group">
          <label>ราคา (บาท) *</label>
          <input type="number" id="f-price" placeholder="0" min="0" step="1">
        </div>
        <div class="form-group">
          <label>หมวดหมู่</label>
          <input type="text" id="f-cat" placeholder="เช่น เสื้อผ้า, อาหาร">
        </div>
        <div class="form-group full">
          <label>รายละเอียดสินค้า</label>
          <textarea id="f-desc" placeholder="บรรยายสินค้าสั้น ๆ"></textarea>
        </div>
        <div class="form-group full">
          <label>รูปสินค้า (เพิ่มได้หลายรูป)</label>
          <div class="image-area">
            <input type="file" id="f-images" accept="image/*" multiple onchange="previewImages(this)">
            <i class="ti ti-photo-plus"></i>
            <span>คลิกหรือลากรูปมาวางที่นี่</span>
            <span style="font-size:11px;">รองรับ JPG, PNG, WEBP</span>
          </div>
          <div id="img-preview" style="display:flex; flex-wrap:wrap; gap:8px; margin-top:10px;"></div>
        </div>
        <div class="form-group">
          <label>สถานะ</label>
          <select id="f-status">
            <option value="active">แสดงในร้าน</option>
            <option value="inactive">ซ่อน</option>
          </select>
        </div>
      </div>
      <div class="actions">
        <button class="btn btn-outline" onclick="resetForm()">ล้างฟอร์ม</button>
        <button class="btn btn-primary" onclick="saveProduct()" id="save-btn">
          <i class="ti ti-device-floppy"></i> บันทึกสินค้า
        </button>
      </div>
    </div>
  </div>

  <!-- TAB: SETTINGS -->
  <div id="tab-settings" style="display:none;">
    <div class="card">
      <h2><i class="ti ti-api"></i> Google Apps Script URL</h2>
      <div class="api-note" id="api-url-display">กำลังโหลด...</div>
      <div class="form-group">
        <label>เปลี่ยน URL (ถ้า Deploy ใหม่)</label>
        <div style="display:flex;gap:10px;">
          <input type="text" id="new-api-url" placeholder="https://script.google.com/macros/s/.../exec">
          <button class="btn btn-primary btn-sm" onclick="saveApiUrl()">
            <i class="ti ti-device-floppy"></i> บันทึก
          </button>
        </div>
      </div>
    </div>

    <div class="card">
      <h2><i class="ti ti-brand-line"></i> ตั้งค่า LINE</h2>
      <div class="form-group" style="margin-bottom:1rem;">
        <label>LINE ID ของร้าน</label>
        <div class="line-config">
          <input type="text" id="line-id" placeholder="เช่น @myshop">
          <button class="line-btn" onclick="saveSettings()">
            <i class="ti ti-device-floppy"></i> บันทึก
          </button>
        </div>
      </div>
    </div>

    <div class="card">
      <h2><i class="ti ti-info-circle"></i> ข้อมูลร้าน</h2>
      <div class="form-grid">
        <div class="form-group">
          <label>ชื่อร้าน</label>
          <input type="text" id="shop-name" placeholder="ชื่อร้านของคุณ">
        </div>
        <div class="form-group">
          <label>สโลแกน</label>
          <input type="text" id="shop-slogan" placeholder="ประโยคสั้น ๆ แนะนำร้าน">
        </div>
      </div>
      <div class="actions">
        <button class="btn btn-primary" onclick="saveSettings()">
          <i class="ti ti-device-floppy"></i> บันทึกทั้งหมด
        </button>
      </div>
    </div>
  </div>

</div><!-- /container -->

<div class="toast" id="toast"><i class="ti ti-check"></i><span id="toast-msg">บันทึกเรียบร้อย</span></div>

<script>
const DEFAULT_API = 'https://script.google.com/macros/s/AKfycbzdhbmv2UnAN0sgM5TiIoEBBiol2OqOSq4LMqwg5bAr7ADZo1iGi34W7deM6sRpVjr2EA/exec';
let products = [];
let imageBuffers = {};

function getApi() { return localStorage.getItem('shop_api_url') || DEFAULT_API; }

async function apiFetch(params) {
  const url = getApi() + '?action=' + params.action + '&t=' + Date.now();
  if (params.method === 'POST') {
    const r = await fetch(url, { method: 'POST', body: JSON.stringify(params.body) });
    return r.json();
  } else {
    const r = await fetch(url);
    return r.json();
  }
}

async function loadProducts() {
  document.getElementById('loading-msg').style.display = 'block';
  document.getElementById('product-table').style.display = 'none';
  document.getElementById('empty-msg').style.display = 'none';
  document.getElementById('stat-sync').textContent = '⏳';
  try {
    const res = await apiFetch({ action: 'getProducts' });
    products = res.products || [];
    renderTable();
    document.getElementById('stat-sync').textContent = '✓';
  } catch(e) {
    showToast('เชื่อมต่อ Google Sheets ไม่ได้ ลองใหม่อีกครั้ง', true);
    document.getElementById('stat-sync').textContent = '✗';
    document.getElementById('loading-msg').style.display = 'none';
  }
}

function renderTable() {
  document.getElementById('loading-msg').style.display = 'none';
  const tbody = document.getElementById('product-tbody');
  const table = document.getElementById('product-table');
  const empty = document.getElementById('empty-msg');
  document.getElementById('stat-total').textContent = products.length;
  document.getElementById('stat-active').textContent = products.filter(p => p.status === 'active').length;

  if (products.length === 0) {
    table.style.display = 'none'; empty.style.display = 'block';
  } else {
    table.style.display = ''; empty.style.display = 'none';
    tbody.innerHTML = products.map((p, i) => {
      const imgs = Array.isArray(p.images) ? p.images : [];
      const imgSrc = imgs[0] || null;
      return `<tr>
        <td>${imgSrc ? `<img src="${imgSrc}" class="thumb">` : `<div class="thumb-placeholder"><i class="ti ti-photo-off"></i></div>`}</td>
        <td><strong>${escHtml(p.name)}</strong>${p.desc ? `<br><span style="font-size:12px;color:#aaa;">${escHtml(String(p.desc).slice(0, 40))}${String(p.desc).length > 40 ? '…' : ''}</span>` : ''}</td>
        <td class="price">฿${Number(p.price).toLocaleString()}</td>
        <td style="color:#888;font-size:13px;">${escHtml(p.cat || '—')}</td>
        <td><span class="badge ${p.status === 'active' ? 'badge-active' : 'badge-inactive'}">${p.status === 'active' ? 'แสดง' : 'ซ่อน'}</span></td>
        <td>
          <button class="btn btn-sm btn-outline" onclick="editProduct(${i})" style="margin-right:4px;"><i class="ti ti-edit"></i></button>
          <button class="btn btn-sm btn-danger" onclick="deleteProduct('${escHtml(String(p.id))}')"><i class="ti ti-trash"></i></button>
        </td>
      </tr>`;
    }).join('');
  }
}

function previewImages(input) {
  const preview = document.getElementById('img-preview');
  preview.innerHTML = '';
  imageBuffers = {};
  Array.from(input.files).forEach((file, idx) => {
    const reader = new FileReader();
    reader.onload = e => {
      imageBuffers[idx] = e.target.result;
      const wrapper = document.createElement('div');
      wrapper.style.cssText = 'position:relative;display:inline-block;';
      const img = document.createElement('img');
      img.src = e.target.result;
      img.style.cssText = 'width:70px;height:70px;object-fit:cover;border-radius:8px;border:1px solid #ddd;';
      const del = document.createElement('button');
      del.innerHTML = '<i class="ti ti-x" style="font-size:10px;"></i>';
      del.style.cssText = 'position:absolute;top:-4px;right:-4px;background:#ef4444;color:white;border:none;border-radius:50%;width:18px;height:18px;cursor:pointer;display:flex;align-items:center;justify-content:center;padding:0;';
      del.onclick = () => { delete imageBuffers[idx]; wrapper.remove(); };
      wrapper.appendChild(img); wrapper.appendChild(del); preview.appendChild(wrapper);
    };
    reader.readAsDataURL(file);
  });
}

async function saveProduct() {
  const name = document.getElementById('f-name').value.trim();
  const price = document.getElementById('f-price').value.trim();
  if (!name || !price) { showToast('กรุณากรอกชื่อสินค้าและราคา', true); return; }
  const editId = document.getElementById('edit-id').value;
  const newImages = Object.values(imageBuffers);
  const existingImages = editId ? ((products.find(p => String(p.id) === editId) || {}).images || []) : [];
  const product = {
    id: editId || Date.now().toString(),
    name, price: parseFloat(price),
    cat: document.getElementById('f-cat').value.trim(),
    desc: document.getElementById('f-desc').value.trim(),
    status: document.getElementById('f-status').value,
    images: newImages.length > 0 ? newImages : existingImages,
    updatedAt: Date.now()
  };
  const btn = document.getElementById('save-btn');
  btn.innerHTML = '<i class="ti ti-loader"></i> กำลังบันทึก...'; btn.disabled = true;
  try {
    await apiFetch({ action: 'saveProduct', method: 'POST', body: { action: 'saveProduct', product } });
    showToast('บันทึกสินค้าเรียบร้อย ✓');
    resetForm(); showTab('products'); await loadProducts();
  } catch(e) {
    showToast('บันทึกไม่สำเร็จ ลองใหม่อีกครั้ง', true);
  }
  btn.innerHTML = '<i class="ti ti-device-floppy"></i> บันทึกสินค้า'; btn.disabled = false;
}

function editProduct(i) {
  const p = products[i];
  document.getElementById('edit-id').value = p.id;
  document.getElementById('f-name').value = p.name;
  document.getElementById('f-price').value = p.price;
  document.getElementById('f-cat').value = p.cat || '';
  document.getElementById('f-desc').value = p.desc || '';
  document.getElementById('f-status').value = p.status || 'active';
  document.getElementById('form-title').textContent = 'แก้ไขสินค้า';
  const preview = document.getElementById('img-preview');
  preview.innerHTML = ''; imageBuffers = {};
  const imgs = Array.isArray(p.images) ? p.images : [];
  imgs.forEach((src, idx) => {
    imageBuffers[idx] = src;
    const wrapper = document.createElement('div');
    wrapper.style.cssText = 'position:relative;display:inline-block;';
    const img = document.createElement('img');
    img.src = src;
    img.style.cssText = 'width:70px;height:70px;object-fit:cover;border-radius:8px;border:1px solid #ddd;';
    const del = document.createElement('button');
    del.innerHTML = '<i class="ti ti-x" style="font-size:10px;"></i>';
    del.style.cssText = 'position:absolute;top:-4px;right:-4px;background:#ef4444;color:white;border:none;border-radius:50%;width:18px;height:18px;cursor:pointer;display:flex;align-items:center;justify-content:center;padding:0;';
    del.onclick = () => { delete imageBuffers[idx]; wrapper.remove(); };
    wrapper.appendChild(img); wrapper.appendChild(del); preview.appendChild(wrapper);
  });
  showTab('add');
}

async function deleteProduct(id) {
  if (!confirm('ลบสินค้านี้?')) return;
  try {
    await apiFetch({ action: 'deleteProduct', method: 'POST', body: { action: 'deleteProduct', id } });
    showToast('ลบสินค้าแล้ว'); await loadProducts();
  } catch(e) {
    showToast('ลบไม่สำเร็จ', true);
  }
}

function resetForm() {
  ['edit-id', 'f-name', 'f-price', 'f-cat', 'f-desc'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('f-status').value = 'active';
  document.getElementById('f-images').value = '';
  document.getElementById('img-preview').innerHTML = '';
  document.getElementById('form-title').textContent = 'เพิ่มสินค้าใหม่';
  imageBuffers = {};
}

async function loadSettings() {
  document.getElementById('api-url-display').textContent = getApi();
  try {
    const res = await apiFetch({ action: 'getSettings' });
    const s = res.settings || {};
    document.getElementById('line-id').value = s.shop_line || '';
    document.getElementById('shop-name').value = s.shop_name || '';
    document.getElementById('shop-slogan').value = s.shop_slogan || '';
  } catch(e) {}
}

async function saveSettings() {
  const settings = {
    shop_line: document.getElementById('line-id').value.trim(),
    shop_name: document.getElementById('shop-name').value.trim(),
    shop_slogan: document.getElementById('shop-slogan').value.trim(),
  };
  try {
    await apiFetch({ action: 'saveSettings', method: 'POST', body: { action: 'saveSettings', settings } });
    showToast('บันทึกการตั้งค่าเรียบร้อย ✓');
  } catch(e) {
    showToast('บันทึกไม่สำเร็จ', true);
  }
}

function saveApiUrl() {
  const val = document.getElementById('new-api-url').value.trim();
  if (!val) return;
  localStorage.setItem('shop_api_url', val);
  document.getElementById('api-url-display').textContent = val;
  showToast('บันทึก URL แล้ว ✓');
}

function showTab(name) {
  ['products', 'add', 'settings'].forEach(t => {
    document.getElementById('tab-' + t).style.display = t === name ? '' : 'none';
  });
  document.querySelectorAll('.tab').forEach((el, i) => {
    el.classList.toggle('active', ['products', 'add', 'settings'][i] === name);
  });
  if (name === 'products') loadProducts();
  if (name === 'settings') loadSettings();
}

function showToast(msg, isErr) {
  const t = document.getElementById('toast');
  document.getElementById('toast-msg').textContent = msg;
  t.style.background = isErr ? '#dc2626' : '#1a1a2e';
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

function escHtml(s) { return String(s || '').replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;'); }

loadProducts();
</script>
</body>
</html>
