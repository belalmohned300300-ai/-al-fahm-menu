<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لوحة تحكم مطعم ع الفحم</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap" rel="stylesheet">
  <style> body { font-family: 'Cairo', sans-serif; background-color: #09090b; color: #f4f4f5; } </style>
</head>
<body class="min-h-screen pb-20">

  <!-- Login Screen -->
  <div id="loginScreen" class="fixed inset-0 z-50 bg-black flex items-center justify-center p-4">
    <div class="bg-neutral-900 border border-neutral-800 w-full max-w-sm rounded-2xl p-6 text-center space-y-4 shadow-2xl">
      <div class="w-16 h-16 bg-gradient-to-tr from-red-600 to-amber-500 rounded-full flex items-center justify-center text-3xl mx-auto shadow-lg shadow-red-900/40">🔥</div>
      <div>
        <h2 class="text-xl font-extrabold text-white">لوحة تحكم "ع الفحم"</h2>
        <p class="text-xs text-neutral-400 mt-1">خاصة بصاحب المكان والمدير فقط</p>
      </div>
      <input type="password" id="passInput" placeholder="أدخل كلمة السر" class="w-full bg-black border border-neutral-800 rounded-xl px-4 py-3 text-center text-white focus:outline-none focus:border-amber-500">
      <button onclick="login()" class="w-full bg-gradient-to-r from-red-600 to-amber-600 text-white font-bold py-3 rounded-xl shadow-lg hover:opacity-90 transition">دخول لوحة التحكم</button>
    </div>
  </div>

  <!-- Dashboard Content -->
  <div id="dashContent" class="hidden">
    <!-- Navbar -->
    <header class="sticky top-0 z-30 bg-neutral-900/90 backdrop-blur-md border-b border-neutral-800 px-4 py-3">
      <div class="max-w-4xl mx-auto flex items-center justify-between">
        <div class="flex items-center gap-2">
          <span class="text-2xl">🔥</span>
          <h1 class="font-extrabold text-lg text-white">إدارة مطعم ع الفحم</h1>
        </div>
        <button onclick="logout()" class="text-xs bg-red-950 text-red-400 border border-red-800 px-3 py-1.5 rounded-lg font-bold hover:bg-red-900">تسجيل خروج</button>
      </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 pt-6 space-y-6">

      <!-- Status & Stats Card -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <!-- Restaurant Status Toggle -->
        <div class="bg-neutral-900 border border-neutral-800 p-4 rounded-2xl space-y-3 md:col-span-1">
          <h3 class="font-bold text-sm text-neutral-400 flex items-center gap-2">
            <i class="fa-solid fa-store text-amber-500"></i> حالة المطعم الآن
          </h3>
          <div class="flex items-center justify-between">
            <span id="statusBadge" class="px-3 py-1 rounded-full text-xs font-black">جاري التحميل...</span>
            <button onclick="toggleRestaurantStatus()" class="bg-neutral-800 border border-neutral-700 text-xs px-3 py-1.5 rounded-xl font-bold hover:bg-neutral-700">تغيير الحالة</button>
          </div>
          <input type="text" id="statusMsgInput" onchange="updateStatusMsg()" placeholder="رسالة الإغلاق للعملاء..." class="w-full bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
        </div>

        <!-- Orders & Revenue Stats -->
        <div class="bg-neutral-900 border border-neutral-800 p-4 rounded-2xl flex flex-col justify-between">
          <div class="text-xs text-neutral-400 font-bold">إجمالي طلبات اليوم</div>
          <div class="text-3xl font-black text-amber-500" id="statOrdersCount">0</div>
          <div class="text-[10px] text-neutral-500">يتحدد تلقائياً مع طلبات الواتساب</div>
        </div>

        <div class="bg-neutral-900 border border-neutral-800 p-4 rounded-2xl flex flex-col justify-between">
          <div class="text-xs text-neutral-400 font-bold">إجمالي المبيعات التقديرية</div>
          <div class="text-3xl font-black text-emerald-500"><span id="statTotalSales">0</span> ج.م</div>
          <div class="text-[10px] text-neutral-500">مجموع المبالغ المطلوبة</div>
        </div>
      </div>

      <!-- Inventory & Price Control Section -->
      <div class="bg-neutral-900 border border-neutral-800 rounded-2xl p-5 space-y-4">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-2 border-b border-neutral-800 pb-3">
          <h2 class="font-bold text-base text-white flex items-center gap-2">
            <i class="fa-solid fa-boxes-stacked text-amber-500"></i> إدارة المخزن والأسعار والأصناف
          </h2>
          <input type="text" id="searchAdminItem" oninput="renderAdminItems()" placeholder="بحث عن صنف..." class="bg-black border border-neutral-800 rounded-xl px-3 py-1.5 text-xs text-white focus:outline-none">
        </div>

        <div id="adminItemsList" class="space-y-3 max-h-96 overflow-y-auto pr-1">
          <!-- Dynamic Items List -->
        </div>
      </div>

      <!-- Add New Item & Category Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Add Item -->
        <div class="bg-neutral-900 border border-neutral-800 p-5 rounded-2xl space-y-3">
          <h3 class="font-bold text-sm text-amber-500 flex items-center gap-2">
            <i class="fa-solid fa-plus-circle"></i> إضافة صنف جديد
          </h3>
          <input type="text" id="newItemName" placeholder="اسم الصنف" class="w-full bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
          <div class="flex gap-2">
            <input type="number" id="newItemPrice" placeholder="السعر (ج.م)" class="w-1/2 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
            <input type="number" id="newItemStock" placeholder="الكمية بالمخزن" class="w-1/2 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
          </div>
          <select id="newItemCatSelect" class="w-full bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white"></select>
          <button onclick="addNewItem()" class="w-full bg-emerald-600 hover:bg-emerald-500 font-bold py-2.5 rounded-xl text-xs transition">حفظ وإضافة للمنيو</button>
        </div>

        <!-- Add Category & Promos -->
        <div class="space-y-6">
          <!-- Add Category -->
          <div class="bg-neutral-900 border border-neutral-800 p-5 rounded-2xl space-y-3">
            <h3 class="font-bold text-sm text-amber-500 flex items-center gap-2">
              <i class="fa-solid fa-folder-plus"></i> إضافة قسم جديد للمنيو
            </h3>
            <div class="flex gap-2">
              <input type="text" id="newCatName" placeholder="اسم القسم الجديد" class="flex-1 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
              <button onclick="addNewCategory()" class="bg-amber-600 hover:bg-amber-500 font-bold px-4 py-2 rounded-xl text-xs">إضافة</button>
            </div>
          </div>

          <!-- Add Promo Code -->
          <div class="bg-neutral-900 border border-neutral-800 p-5 rounded-2xl space-y-3">
            <h3 class="font-bold text-sm text-amber-500 flex items-center gap-2">
              <i class="fa-solid fa-ticket"></i> إضافة كود خصم
            </h3>
            <div class="flex gap-2">
              <input type="text" id="promoCodeName" placeholder="الكود (مثال: OFF10)" class="uppercase flex-1 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
              <input type="number" id="promoPercent" placeholder="الخصم %" class="w-24 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
              <button onclick="addPromoCode()" class="bg-emerald-600 hover:bg-emerald-500 font-bold px-4 py-2 rounded-xl text-xs">تفعيل</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Change Admin Password -->
      <div class="bg-neutral-900 border border-neutral-800 p-5 rounded-2xl space-y-3">
        <h3 class="font-bold text-sm text-neutral-300 flex items-center gap-2">
          <i class="fa-solid fa-key text-amber-500"></i> تغيير كلمة سر لوحة التحكم
        </h3>
        <div class="flex gap-2 max-w-md">
          <input type="password" id="newPassInput" placeholder="كلمة السر الجديدة" class="flex-1 bg-black border border-neutral-800 rounded-xl px-3 py-2 text-xs text-white">
          <button onclick="changePassword()" class="bg-neutral-800 hover:bg-neutral-700 text-amber-500 border border-neutral-700 font-bold px-4 py-2 rounded-xl text-xs">تحديث السر</button>
        </div>
      </div>

    </main>
  </div>

  <script>
    const defaultCategories = ["الطواجن الرئيسية", "المكرونات", "طواجن موتزريلا", "الفته", "الوجبات", "وجبات الميكس", "المحشي", "المشويات الرئيسية", "حواوشي كبير ع الفحم"];
    
    let categories = JSON.parse(localStorage.getItem('alfahm_cats')) || defaultCategories;
    let menuItems = JSON.parse(localStorage.getItem('alfahm_menu')) || [];
    let promoCodes = JSON.parse(localStorage.getItem('alfahm_promos')) || { 'OFF10': 10 };
    let adminPass = localStorage.getItem('alfahm_pass') || '1234';
    let isStoreOpen = JSON.parse(localStorage.getItem('alfahm_open')) ?? true;
    let storeMsg = localStorage.getItem('alfahm_msg') || 'المطعم مغلق حالياً، نسعد باستقبال طلباتكم في أوقات العمل!';
    let stats = JSON.parse(localStorage.getItem('alfahm_stats')) || { orders: 0, sales: 0 };

    function login() {
      const input = document.getElementById('passInput').value;
      if (input === adminPass) {
        document.getElementById('loginScreen').classList.add('hidden');
        document.getElementById('dashContent').classList.remove('hidden');
        initDash();
      } else {
        alert('كلمة السر غير صحيحة!');
      }
    }

    function logout() {
      location.reload();
    }

    function initDash() {
      renderStatus();
      renderStats();
      renderCategorySelect();
      renderAdminItems();
    }

    function renderStatus() {
      const badge = document.getElementById('statusBadge');
      if (isStoreOpen) {
        badge.className = "px-3 py-1 rounded-full text-xs font-black bg-emerald-950 text-emerald-400 border border-emerald-800";
        badge.innerText = "المطعم مفتوح الآن";
      } else {
        badge.className = "px-3 py-1 rounded-full text-xs font-black bg-red-950 text-red-400 border border-red-800";
        badge.innerText = "المطعم مغلق حالياً";
      }
      document.getElementById('statusMsgInput').value = storeMsg;
    }

    function toggleRestaurantStatus() {
      isStoreOpen = !isStoreOpen;
      localStorage.setItem('alfahm_open', JSON.stringify(isStoreOpen));
      renderStatus();
    }

    function updateStatusMsg() {
      storeMsg = document.getElementById('statusMsgInput').value;
      localStorage.setItem('alfahm_msg', storeMsg);
      alert('تم حفظ الرسالة التنبيهية للعملاء');
    }

    function renderStats() {
      document.getElementById('statOrdersCount').innerText = stats.orders;
      document.getElementById('statTotalSales').innerText = stats.sales;
    }

    function renderCategorySelect() {
      const select = document.getElementById('newItemCatSelect');
      select.innerHTML = categories.map(c => `<option value="${c}">${c}</option>`).join('');
    }

    function renderAdminItems() {
      const container = document.getElementById('adminItemsList');
      const search = document.getElementById('searchAdminItem').value.trim().toLowerCase();

      let filtered = menuItems.filter(i => i.name.toLowerCase().includes(search));

      container.innerHTML = filtered.map(item => `
        <div class="bg-black border border-neutral-800 p-3 rounded-xl flex flex-col md:flex-row md:items-center justify-between gap-3 text-xs">
          <div class="space-y-1 flex-1">
            <div class="font-bold text-white text-sm">${item.name} <span class="text-[10px] text-amber-500 font-normal">(${item.cat})</span></div>
            <div class="flex items-center gap-2 text-neutral-400">
              <span>السعر:</span>
              <input type="number" value="${item.price}" onchange="updatePrice(${item.id}, this.value)" class="w-16 bg-neutral-900 border border-neutral-700 text-center text-amber-500 rounded py-0.5 font-bold">
              <span>ج.م</span>
            </div>
          </div>

          <!-- Inventory Control -->
          <div class="flex items-center gap-2 bg-neutral-900 p-1.5 rounded-lg border border-neutral-800">
            <span class="text-neutral-400 font-bold">المخزون:</span>
            <input type="number" value="${item.stock ?? 50}" onchange="updateStock(${item.id}, this.value)" class="w-14 bg-black border border-neutral-700 text-center text-emerald-400 rounded py-0.5 font-bold">
            <button onclick="addStock(${item.id}, 10)" class="bg-emerald-950 hover:bg-emerald-900 text-emerald-400 border border-emerald-800 px-2 py-0.5 rounded font-bold">+10 توريد</button>
          </div>

          <!-- Availability Toggle -->
          <button onclick="toggleAvailability(${item.id})" class="px-3 py-1.5 rounded-lg font-bold border ${item.available ? 'bg-emerald-950 text-emerald-400 border-emerald-800' : 'bg-red-950 text-red-400 border-red-800'}">
            ${item.available ? 'متاح' : 'مغلق'}
          </button>
        </div>
      `).join('');
    }

    function updatePrice(id, val) {
      let item = menuItems.find(i => i.id == id);
      if (item) { item.price = parseFloat(val) || item.price; saveData(); }
    }

    function updateStock(id, val) {
      let item = menuItems.find(i => i.id == id);
      if (item) { 
        item.stock = parseInt(val) || 0; 
        if (item.stock <= 0) item.available = false;
        saveData(); 
        renderAdminItems();
      }
    }

    function addStock(id, amount) {
      let item = menuItems.find(i => i.id == id);
      if (item) { 
        item.stock = (item.stock || 0) + amount; 
        if (item.stock > 0) item.available = true;
        saveData(); 
        renderAdminItems();
      }
    }

    function toggleAvailability(id) {
      let item = menuItems.find(i => i.id == id);
      if (item) { item.available = !item.available; saveData(); renderAdminItems(); }
    }

    function addNewItem() {
      const name = document.getElementById('newItemName').value.trim();
      const price = parseFloat(document.getElementById('newItemPrice').value);
      const stock = parseInt(document.getElementById('newItemStock').value) || 50;
      const cat = document.getElementById('newItemCatSelect').value;

      if (name && price) {
        menuItems.push({ id: Date.now(), name, price, stock, cat, available: true });
        saveData();
        renderAdminItems();
        document.getElementById('newItemName').value = '';
        document.getElementById('newItemPrice').value = '';
        alert('تم إضافة الصنف للمخزن والمنيو بنجاح!');
      }
    }

    function addNewCategory() {
      const name = document.getElementById('newCatName').value.trim();
      if (name && !categories.includes(name)) {
        categories.push(name);
        localStorage.setItem('alfahm_cats', JSON.stringify(categories));
        renderCategorySelect();
        document.getElementById('newCatName').value = '';
        alert('تم إضافة القسم الجديد بنجاح!');
      }
    }

    function addPromoCode() {
      const code = document.getElementById('promoCodeName').value.trim().toUpperCase();
      const percent = parseFloat(document.getElementById('promoPercent').value);
      if (code && percent) {
        promoCodes[code] = percent;
        localStorage.setItem('alfahm_promos', JSON.stringify(promoCodes));
        document.getElementById('promoCodeName').value = '';
        document.getElementById('promoPercent').value = '';
        alert(`تم تفعيل كود الخصم ${code} بنسبة ${percent}%`);
      }
    }

    function changePassword() {
      const newP = document.getElementById('newPassInput').value.trim();
      if (newP) {
        adminPass = newP;
        localStorage.setItem('alfahm_pass', adminPass);
        alert('تم تغيير كلمة سر لوحة التحكم بنجاح!');
      }
    }

    function saveData() {
      localStorage.setItem('alfahm_menu', JSON.stringify(menuItems));
    }
  </script>
</body>
</html>
