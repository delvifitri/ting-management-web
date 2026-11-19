<script setup>
import { ref, computed } from 'vue'

// ================= STATE =================
const currentTab = ref('active') // 'active' | 'catalog' | 'history'
const isSidebarOpen = ref(true)
const isAdvancedMode = ref(false)
const isProfileDropdownOpen = ref(false)
const catalogCategory = ref('all') // 'all' | 'pos' | 'ai' | 'enterprise'

// Modals
const isReceiptModalOpen = ref(false)
const selectedReceipt = ref(null)
const isCheckoutModalOpen = ref(false)
const selectedPlanToBuy = ref(null)

// Toast
const toastMessage = ref('')
const isToastVisible = ref(false)
let toastTimer = null

function showToast(message) {
  toastMessage.value = message
  isToastVisible.value = true
  if (toastTimer) clearTimeout(toastTimer)
  toastTimer = setTimeout(() => {
    isToastVisible.value = false
  }, 2500)
}

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value
}

function toggleProfileDropdown() {
  isProfileDropdownOpen.value = !isProfileDropdownOpen.value
}

function handleAdvanceToggle() {
  showToast(isAdvancedMode.value ? 'Mode dialihkan ke TING Advance' : 'Mode standar aktif')
}

// ================= KATALOG DATA =================
const catalogPlans = [
  {
    id: 'ting-advance',
    name: 'TING Advance',
    badge: 'POPULER',
    category: 'pos',
    categoryLabel: 'KASIR & POS',
    subTitle: 'Retail & Multi-Cabang',
    icon: '🏪',
    gradientClass: 'from-sky-600 to-cyan-500',
    capacity: 'Unlimited Produk & Multi-Kasir',
    purpose: 'Kelola kasir dan stok cabang tanpa selisih, laporan keuangan otomatis, dan cetak struk profesional.',
    price: 99000,
    priceFormatted: 'Rp 99.000',
    period: '/ bulan',
    features: [
      'Unlimited transaksi kasir & barcode',
      'Manajemen stok multi-cabang realtime',
      'Laporan laba kotor & arus kas otomatis',
      'Support printer thermal Bluetooth & USB',
      'Multi-role karyawan (kasir, supervisor, owner)'
    ]
  },
  {
    id: 'ai-ting-assistant',
    name: 'AI TING Assistant',
    badge: 'SMART AI',
    category: 'ai',
    categoryLabel: 'AI & OTOMASI',
    subTitle: 'Smart Insights & WhatsApp Bot',
    icon: '🤖',
    gradientClass: 'from-purple-600 to-indigo-600',
    capacity: '3,000 AI Token / Bulan',
    purpose: 'Analisis produk laris otomatis, peringatan restock harian, dan bot pemesanan via WhatsApp.',
    price: 149000,
    priceFormatted: 'Rp 149.000',
    period: '/ bulan',
    features: [
      '3.000 AI Token per bulan',
      'Analisis prediksi restock & tren penjualan',
      'Bot WhatsApp auto-reply katalog menu',
      'Rekomendasi promo cerdas berbasis riwayat belanja',
      'Laporan performa harian dikirim via WhatsApp'
    ]
  },
  {
    id: 'enterprise-support',
    name: 'Enterprise Support',
    badge: 'KORPORASI',
    category: 'enterprise',
    categoryLabel: 'ENTERPRISE',
    subTitle: 'Custom SLA & API ERP',
    icon: '🏢',
    gradientClass: 'from-slate-800 to-slate-950',
    capacity: 'Unlimited Server & Cabang',
    purpose: 'Kustomisasi API integrasi, kunjungan teknis on-site, serta Dedicated Account Manager 24/7.',
    price: 499000,
    priceFormatted: 'Rp 499.000',
    period: '/ bulan',
    features: [
      'Dedicated Account Manager & Engineer',
      'Kustomisasi integrasi ERP & Webhook API',
      'SLA Uptime 99.9% dengan garansi teknis',
      'Kunjungan teknis & training on-site',
      'Custom branding & white-label receipt'
    ]
  }
]

const filteredCatalog = computed(() => {
  if (catalogCategory.value === 'all') return catalogPlans
  return catalogPlans.filter(p => p.category === catalogCategory.value)
})

// ================= DATA RIWAYAT TRANSAKSI DUMMY =================
const transactions = [
  {
    id: 'SUB-20251109-01',
    orderId: 'SUB-TSA-20251109-0001',
    name: 'Testing System Analyst',
    period: '9 November 2025 - 9 November 2026',
    amount: 'Rp 1',
    method: 'QRIS',
    date: '9 Nov 2025, 15.06 WIB',
    isPaid: true,
    isActivePlan: false
  },
  {
    id: 'SUB-20251109-02',
    orderId: 'SUB-TSA-20251109-0002',
    name: 'Testing System Analyst',
    period: '9 November 2025 - 9 November 2027',
    amount: 'Rp 1',
    method: 'QRIS',
    date: '9 Nov 2025, 13.55 WIB',
    isPaid: true,
    isActivePlan: true // Muncul badge hijau ACTIVE PLAN
  },
  {
    id: 'SUB-20251017-01',
    orderId: 'SUB-TSA-20251017-0001',
    name: 'Testing System Analyst',
    period: '17 Oktober 2025 - 17 Oktober 2026',
    amount: 'Rp 1',
    method: 'QRIS',
    date: '17 Okt 2025, 14.10 WIB',
    isPaid: true,
    isActivePlan: false
  }
]

// ================= ACTIONS =================
function openReceiptModal(tx) {
  selectedReceipt.value = tx
  isReceiptModalOpen.value = true
}

function closeReceiptModal() {
  isReceiptModalOpen.value = false
}

function printReceipt() {
  window.print()
}

function openCheckoutModal(plan) {
  selectedPlanToBuy.value = plan
  isCheckoutModalOpen.value = true
}

function closeCheckoutModal() {
  isCheckoutModalOpen.value = false
}

function processPayment() {
  showToast(`Pembayaran untuk ${selectedPlanToBuy.value.name} berhasil diproses!`)
  isCheckoutModalOpen.value = false
}
</script>

<template>
  <div class="min-h-screen flex flex-col bg-[#F8FAFC] text-slate-800 antialiased font-sans">
    
    <!-- ================= TOP NAVBAR TING MANAGEMENT ================= -->
    <header class="h-16 bg-white border-b border-slate-200 px-4 md:px-6 flex items-center justify-between sticky top-0 z-40 select-none">
      <div class="flex items-center gap-4">
        <button 
          type="button"
          @click="toggleSidebar" 
          class="p-2 text-slate-600 hover:text-sky-600 hover:bg-slate-100 rounded-lg transition-colors cursor-pointer" 
          title="Toggle Sidebar"
        >
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
          </svg>
        </button>

        <!-- TING Brand Logo -->
        <div class="flex items-center gap-2 cursor-pointer">
          <div class="w-7 h-7 bg-sky-500 rounded-lg flex items-center justify-center text-white font-black text-sm shadow-sm shadow-sky-200">
            <svg class="w-4 h-4" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
            </svg>
          </div>
          <span class="text-xl font-extrabold tracking-tight text-sky-600">TING</span>
        </div>
      </div>

      <!-- Right Controls -->
      <div class="flex items-center gap-3">
        <!-- Store Switcher: Toko Pak Bagus -->
        <div class="hidden sm:flex items-center gap-2 border border-slate-200 px-3 py-1.5 rounded-lg text-xs font-medium text-slate-700 bg-white hover:bg-slate-50 cursor-pointer shadow-2xs">
          <span>Toko Pak Bagus</span>
          <svg class="w-3.5 h-3.5 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/>
          </svg>
        </div>

        <!-- Unit Bisnis Switcher -->
        <div class="hidden sm:flex items-center gap-2 border border-slate-200 px-3 py-1.5 rounded-lg text-xs font-medium text-slate-700 bg-white hover:bg-slate-50 cursor-pointer shadow-2xs">
          <svg class="w-3.5 h-3.5 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"/>
          </svg>
          <span>Unit Bisnis</span>
          <svg class="w-3.5 h-3.5 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/>
          </svg>
        </div>

        <!-- Coin Balance Badge -->
        <div class="flex items-center gap-1.5 bg-amber-50 border border-amber-200 text-amber-700 px-3 py-1.5 rounded-full text-xs font-bold">
          <span>🪙</span>
          <span>102,446</span>
        </div>

        <!-- Profile Avatar with Dropdown -->
        <div class="relative">
          <button 
            type="button"
            @click="toggleProfileDropdown" 
            class="w-8 h-8 rounded-full bg-purple-700 text-white font-bold text-sm flex items-center justify-center hover:ring-2 hover:ring-purple-300 transition-all cursor-pointer"
          >
            D
          </button>

          <!-- Dropdown Menu Box -->
          <div 
            v-if="isProfileDropdownOpen"
            class="absolute right-0 mt-2 w-64 bg-white rounded-xl shadow-xl border border-slate-200 py-2 z-50 transition-all duration-150"
          >
            <div class="px-4 py-2 border-b border-slate-100">
              <p class="text-xs font-bold text-slate-800">Toko Pak Bagus</p>
              <p class="text-[11px] text-slate-500 truncate">delvi@intelove.com</p>
            </div>

            <div class="py-1 text-xs text-slate-700 font-medium">
              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-slate-50 transition-colors">
                <span class="text-amber-500">☀️</span>
                <span>Tema Terang</span>
              </a>

              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-slate-50 text-slate-400 cursor-not-allowed">
                <span>🏢</span>
                <span>Informasi Bisnis</span>
              </a>

              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-slate-50 transition-colors">
                <span>💳</span>
                <span>Riwayat Top Up</span>
              </a>

              <!-- MENU ITEM: SUBSCRIPTION (ACTIVE IN DROPDOWN) -->
              <div class="px-2 py-0.5">
                <div 
                  @click="currentTab = 'active'; isProfileDropdownOpen = false;"
                  class="flex items-center justify-between px-3 py-2 bg-sky-50 text-sky-700 rounded-lg font-bold border border-sky-200 cursor-pointer"
                >
                  <div class="flex items-center gap-2.5">
                    <span>✨</span>
                    <span>Subscription</span>
                  </div>
                  <span class="text-[10px] bg-sky-600 text-white font-extrabold px-1.5 py-0.5 rounded tracking-wider">NEW</span>
                </div>
              </div>

              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-slate-50 text-emerald-600 transition-colors">
                <span>🤖</span>
                <span>Pengaturan AI TING</span>
              </a>

              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-slate-50 text-purple-600 transition-colors">
                <span>🔄</span>
                <span>Mulai Ulang Tour</span>
              </a>

              <div class="border-t border-slate-100 my-1"></div>

              <a href="#" class="flex items-center gap-3 px-4 py-2 hover:bg-rose-50 text-rose-600 transition-colors">
                <span>🚪</span>
                <span>Logout</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- ================= BODY (SIDEBAR + CONTENT) ================= -->
    <div class="flex-1 flex overflow-hidden">

      <!-- ================= LEFT SIDEBAR TING ================= -->
      <aside 
        v-show="isSidebarOpen"
        class="w-60 bg-white border-r border-slate-200 flex flex-col justify-between p-4 shrink-0 transition-all select-none overflow-y-auto custom-scrollbar"
      >
        <div class="space-y-4">
          <!-- Advanced Mode Toggle -->
          <div class="p-3 bg-slate-50 border border-slate-200 rounded-xl">
            <div class="flex items-center justify-between mb-1">
              <span class="text-xs font-bold text-slate-800">Advanced Mode</span>
              <label class="relative inline-flex items-center cursor-pointer">
                <input 
                  type="checkbox" 
                  v-model="isAdvancedMode" 
                  @change="handleAdvanceToggle" 
                  class="sr-only peer"
                />
                <div class="w-9 h-5 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-sky-500"></div>
              </label>
            </div>
            <p class="text-[11px] text-slate-500">Upgrade fitur pro</p>
          </div>

          <!-- Navigation Links -->
          <nav class="space-y-1 text-xs font-semibold">
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">📊</span>
              <span>Ringkasan</span>
            </a>
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">🖥️</span>
              <span>Kasir</span>
            </a>
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">📦</span>
              <span>Daftar Produk</span>
            </a>
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">📥</span>
              <span>Produk Masuk</span>
            </a>
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">📝</span>
              <span>Catat Biaya</span>
            </a>
            <a href="#" class="flex items-center gap-3 px-3 py-2 text-slate-600 hover:bg-slate-50 hover:text-slate-900 rounded-lg transition-colors">
              <span class="text-slate-400">👥</span>
              <span>Atur Pengguna</span>
            </a>

            <!-- Active Menu: Subscription TING -->
            <div class="flex items-center gap-3 px-3 py-2 bg-sky-50 text-sky-600 font-bold rounded-lg transition-colors">
              <span>✨</span>
              <span>Subscription</span>
            </div>
          </nav>
        </div>

        <!-- Bottom Affiliate Box -->
        <div class="mt-4 p-3 bg-gradient-to-br from-sky-50 to-indigo-50 border border-sky-100 rounded-xl space-y-1 text-xs">
          <div class="font-bold text-sky-900 flex items-center gap-1.5">
            <span>🎁</span>
            <span>Gabung Affiliate TING</span>
          </div>
          <p class="text-[11px] text-slate-500 leading-normal">
            Dapatkan komisi hingga 25% setiap referral paket kasir.
          </p>
          <button 
            type="button"
            @click="showToast('Fitur program affiliate segera dibuka!')"
            class="w-full mt-2 py-1.5 bg-white hover:bg-slate-50 border border-sky-200 text-sky-700 rounded-lg font-bold text-[11px] transition-colors cursor-pointer"
          >
            Pelajari Program
          </button>
        </div>
      </aside>

      <!-- ================= MAIN CONTENT (SUBSCRIPTIONS) ================= -->
      <main class="flex-1 overflow-y-auto custom-scrollbar p-4 md:p-6 lg:p-8 space-y-6">

        <!-- Top Header & Breadcrumb -->
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <div class="flex items-center gap-2">
              <h1 class="text-xl md:text-2xl font-extrabold text-slate-900 tracking-tight">
                Subscription Management
              </h1>
              <span class="text-xs bg-sky-100 text-sky-800 font-bold px-2.5 py-0.5 rounded-full">
                Toko Pak Bagus (ID: TPB-88392)
              </span>
            </div>
            <p class="text-xs md:text-sm text-slate-500 mt-1">
              Kelola lisensi kasir, token AI, kuota cabang, dan invoice resmi.
            </p>
          </div>

          <!-- Quick Action Buttons -->
          <div class="flex items-center gap-2">
            <button 
              type="button"
              @click="currentTab = 'catalog'"
              class="px-4 py-2 bg-sky-600 hover:bg-sky-700 text-white rounded-xl text-xs font-bold shadow-xs hover:shadow-md transition-all flex items-center gap-1.5 cursor-pointer"
            >
              <span>+ Beli / Tambah Paket</span>
            </button>
          </div>
        </div>

        <!-- ================= MAIN TABS NAVIGATION ================= -->
        <div class="flex border-b border-slate-200 text-xs font-bold gap-6">
          <button 
            type="button"
            @click="currentTab = 'active'"
            class="pb-3 border-b-2 flex items-center gap-2 transition-all cursor-pointer"
            :class="currentTab === 'active' ? 'border-sky-600 text-sky-600' : 'border-transparent text-slate-500 hover:text-slate-800'"
          >
            <span>Paket Aktif</span>
            <span class="px-1.5 py-0.5 rounded-full text-[10px]" :class="currentTab === 'active' ? 'bg-sky-100 text-sky-700' : 'bg-slate-100 text-slate-600'">
              2 Aktif
            </span>
          </button>

          <button 
            type="button"
            @click="currentTab = 'catalog'"
            class="pb-3 border-b-2 flex items-center gap-2 transition-all cursor-pointer"
            :class="currentTab === 'catalog' ? 'border-sky-600 text-sky-600' : 'border-transparent text-slate-500 hover:text-slate-800'"
          >
            <span>Katalog Subscription</span>
            <span class="px-1.5 py-0.5 rounded-full text-[10px]" :class="currentTab === 'catalog' ? 'bg-sky-100 text-sky-700' : 'bg-slate-100 text-slate-600'">
              3 Pilihan
            </span>
          </button>

          <button 
            type="button"
            @click="currentTab = 'history'"
            class="pb-3 border-b-2 flex items-center gap-2 transition-all cursor-pointer"
            :class="currentTab === 'history' ? 'border-sky-600 text-sky-600' : 'border-transparent text-slate-500 hover:text-slate-800'"
          >
            <span>Riwayat Pembelian Subscription</span>
            <span class="px-1.5 py-0.5 rounded-full text-[10px]" :class="currentTab === 'history' ? 'bg-sky-100 text-sky-700' : 'bg-slate-100 text-slate-600'">
              3 Transaksi
            </span>
          </button>
        </div>

        <!-- ================= TAB 1: PAKET AKTIF ================= -->
        <div v-if="currentTab === 'active'" class="space-y-6">
          
          <!-- Banner 1: TING Advance -->
          <div class="bg-gradient-to-r from-sky-600 via-sky-500 to-cyan-500 rounded-2xl p-6 text-white shadow-md relative overflow-hidden">
            <div class="relative z-10 flex flex-col lg:flex-row lg:items-center justify-between gap-6">
              <div class="space-y-2 max-w-xl">
                <div class="flex items-center gap-2">
                  <span class="text-2xl">🏪</span>
                  <span class="text-xs font-bold uppercase tracking-wider bg-white/20 px-2.5 py-0.5 rounded-full">
                    PAKET UTAMA (ACTIVE)
                  </span>
                </div>
                <h2 class="text-2xl font-extrabold tracking-tight">TING Advance - Multi Cabang</h2>
                <p class="text-xs text-sky-100 leading-relaxed">
                  Lisensi sistem kasir lengkap untuk operasional toko retail, cetak struk otomatis, integrasi barcode scanner, dan laporan finansial harian.
                </p>
                <div class="flex flex-wrap gap-4 pt-2 text-xs">
                  <div>
                    <span class="text-sky-200 block text-[11px]">Masa Berlaku Hingga:</span>
                    <strong class="font-bold">9 November 2027</strong>
                  </div>
                  <div>
                    <span class="text-sky-200 block text-[11px]">Sisa Waktu:</span>
                    <strong class="font-bold">248 Hari Tersisa</strong>
                  </div>
                  <div>
                    <span class="text-sky-200 block text-[11px]">Status Kuota:</span>
                    <strong class="font-bold">Unlimited Produk & Kasir</strong>
                  </div>
                </div>
              </div>

              <div class="bg-white/10 backdrop-blur-xs rounded-xl p-4 lg:w-72 shrink-0 border border-white/20 space-y-3">
                <div class="flex justify-between text-xs">
                  <span class="text-sky-100">Cabang Aktif</span>
                  <strong class="font-bold">3 / 5 Outlet</strong>
                </div>
                <div class="w-full bg-black/20 h-2 rounded-full overflow-hidden">
                  <div class="bg-white h-full rounded-full" style="width: 60%"></div>
                </div>
                <div class="pt-2 flex gap-2">
                  <button 
                    type="button"
                    @click="openCheckoutModal(catalogPlans[0])"
                    class="w-full py-2 bg-white text-sky-700 hover:bg-sky-50 rounded-lg text-xs font-bold transition-all text-center cursor-pointer shadow-xs"
                  >
                    Perpanjang Lisensi
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Banner 2: AI TING Assistant -->
          <div class="bg-gradient-to-r from-purple-700 via-indigo-600 to-indigo-700 rounded-2xl p-6 text-white shadow-md relative overflow-hidden">
            <div class="relative z-10 flex flex-col lg:flex-row lg:items-center justify-between gap-6">
              <div class="space-y-2 max-w-xl">
                <div class="flex items-center gap-2">
                  <span class="text-2xl">🤖</span>
                  <span class="text-xs font-bold uppercase tracking-wider bg-white/20 px-2.5 py-0.5 rounded-full">
                    ADD-ON AI SMART (ACTIVE)
                  </span>
                </div>
                <h2 class="text-2xl font-extrabold tracking-tight">AI TING Assistant (TiChat Pro)</h2>
                <p class="text-xs text-purple-100 leading-relaxed">
                  Asisten kecerdasan buatan untuk analisis produk terlaris, notifikasi restock otomatis via WhatsApp, dan chatbot rekomendasi menu.
                </p>
                <div class="flex flex-wrap gap-4 pt-2 text-xs">
                  <div>
                    <span class="text-purple-200 block text-[11px]">Masa Berlaku:</span>
                    <strong class="font-bold">7 Desember 2026</strong>
                  </div>
                  <div>
                    <span class="text-purple-200 block text-[11px]">Sisa Hari:</span>
                    <strong class="font-bold text-amber-300">18 Hari Lagi</strong>
                  </div>
                  <div>
                    <span class="text-purple-200 block text-[11px]">Sisa Token AI:</span>
                    <strong class="font-bold">1,840 / 3,000 Token</strong>
                  </div>
                </div>
              </div>

              <div class="bg-white/10 backdrop-blur-xs rounded-xl p-4 lg:w-72 shrink-0 border border-white/20 space-y-3">
                <div class="flex justify-between text-xs">
                  <span class="text-purple-100">Token Terpakai Bulan Ini</span>
                  <strong class="font-bold">61%</strong>
                </div>
                <div class="w-full bg-black/20 h-2 rounded-full overflow-hidden">
                  <div class="bg-amber-300 h-full rounded-full" style="width: 61%"></div>
                </div>
                <div class="pt-2 flex gap-2">
                  <button 
                    type="button"
                    @click="openCheckoutModal(catalogPlans[1])"
                    class="w-full py-2 bg-white text-purple-900 hover:bg-purple-50 rounded-lg text-xs font-bold transition-all text-center cursor-pointer shadow-xs"
                  >
                    Top Up Kuota Token
                  </button>
                </div>
              </div>
            </div>
          </div>

        </div>

        <!-- ================= TAB 2: KATALOG SUBSCRIPTION ================= -->
        <div v-if="currentTab === 'catalog'" class="space-y-6">
          
          <!-- Category Filter Pills -->
          <div class="flex items-center gap-2 overflow-x-auto custom-scrollbar pb-1">
            <button 
              type="button"
              @click="catalogCategory = 'all'"
              class="px-4 py-1.5 rounded-full text-xs font-bold transition-all cursor-pointer"
              :class="catalogCategory === 'all' ? 'bg-sky-600 text-white shadow-xs' : 'bg-white border border-slate-200 text-slate-600 hover:bg-slate-50'"
            >
              Semua Kategori (3)
            </button>
            <button 
              type="button"
              @click="catalogCategory = 'pos'"
              class="px-4 py-1.5 rounded-full text-xs font-bold transition-all cursor-pointer"
              :class="catalogCategory === 'pos' ? 'bg-sky-600 text-white shadow-xs' : 'bg-white border border-slate-200 text-slate-600 hover:bg-slate-50'"
            >
              Kasir & POS
            </button>
            <button 
              type="button"
              @click="catalogCategory = 'ai'"
              class="px-4 py-1.5 rounded-full text-xs font-bold transition-all cursor-pointer"
              :class="catalogCategory === 'ai' ? 'bg-sky-600 text-white shadow-xs' : 'bg-white border border-slate-200 text-slate-600 hover:bg-slate-50'"
            >
              AI Assistant
            </button>
            <button 
              type="button"
              @click="catalogCategory = 'enterprise'"
              class="px-4 py-1.5 rounded-full text-xs font-bold transition-all cursor-pointer"
              :class="catalogCategory === 'enterprise' ? 'bg-sky-600 text-white shadow-xs' : 'bg-white border border-slate-200 text-slate-600 hover:bg-slate-50'"
            >
              Enterprise
            </button>
          </div>

          <!-- Catalog Cards Grid (3 Columns) -->
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div 
              v-for="plan in filteredCatalog" 
              :key="plan.id"
              class="bg-white border border-slate-200 rounded-2xl p-6 shadow-xs hover:shadow-lg transition-all flex flex-col justify-between"
            >
              <div class="space-y-4">
                <!-- Header Card -->
                <div class="flex items-start justify-between">
                  <div 
                    class="w-12 h-12 rounded-xl bg-gradient-to-tr text-white flex items-center justify-center text-2xl shadow-xs"
                    :class="plan.gradientClass"
                  >
                    <span>{{ plan.icon }}</span>
                  </div>
                  <span class="text-[10px] font-extrabold px-2.5 py-0.5 rounded-full bg-sky-50 text-sky-700 border border-sky-200">
                    {{ plan.badge }}
                  </span>
                </div>

                <div>
                  <h3 class="text-lg font-bold text-slate-900">{{ plan.name }}</h3>
                  <p class="text-xs text-sky-600 font-semibold mt-0.5">{{ plan.subTitle }}</p>
                  <p class="text-xs text-slate-500 mt-2 leading-relaxed">{{ plan.purpose }}</p>
                </div>

                <!-- Price Box -->
                <div class="p-3 bg-slate-50 rounded-xl border border-slate-100 flex items-baseline gap-1">
                  <span class="text-2xl font-extrabold text-slate-900">{{ plan.priceFormatted }}</span>
                  <span class="text-xs text-slate-500 font-medium">{{ plan.period }}</span>
                </div>

                <!-- Feature Checkmarks -->
                <div class="space-y-2 pt-2">
                  <div class="text-[11px] font-bold text-slate-700 uppercase tracking-wider">Fitur Termasuk:</div>
                  <ul class="space-y-2 text-xs text-slate-600">
                    <li v-for="(feat, idx) in plan.features" :key="idx" class="flex items-start gap-2">
                      <span class="text-emerald-500 font-bold shrink-0">✓</span>
                      <span>{{ feat }}</span>
                    </li>
                  </ul>
                </div>
              </div>

              <!-- CTA Button -->
              <div class="pt-6 mt-6 border-t border-slate-100">
                <button 
                  type="button"
                  @click="openCheckoutModal(plan)"
                  class="w-full py-2.5 bg-sky-600 hover:bg-sky-700 active:bg-sky-800 text-white rounded-xl text-xs font-bold transition-all shadow-xs flex items-center justify-center gap-1.5 cursor-pointer"
                >
                  <span>Beli / Upgrade Paket</span>
                  <span>→</span>
                </button>
              </div>
            </div>
          </div>

        </div>

        <!-- ================= TAB 3: RIWAYAT PEMBELIAN (PERSIS GAMBAR DUMMY) ================= -->
        <div v-if="currentTab === 'history'" class="space-y-4">
          
          <div class="flex items-center justify-between text-xs text-slate-500 pb-1">
            <span>Menampilkan 3 transaksi pembelian subscription terakhir</span>
            <span class="font-semibold text-slate-700">Akun: Testing SA</span>
          </div>

          <div class="space-y-4">
            <div 
              v-for="tx in transactions" 
              :key="tx.id"
              class="bg-white border border-slate-200 rounded-2xl p-5 shadow-xs hover:shadow-md transition-all flex flex-col md:flex-row md:items-center justify-between gap-4"
            >
              <!-- Info Transaksi Kiri -->
              <div class="space-y-1.5">
                <div class="flex flex-wrap items-center gap-2">
                  <h3 class="text-base font-bold text-slate-900">{{ tx.name }}</h3>
                  
                  <span class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-emerald-50 text-emerald-700 border border-emerald-200">
                    PAID
                  </span>

                  <!-- Badge ACTIVE PLAN (Khusus order ke-2 sesuai screenshot dummy) -->
                  <span 
                    v-if="tx.isActivePlan"
                    class="text-[10px] font-extrabold px-2 py-0.5 rounded-full bg-emerald-600 text-white shadow-2xs tracking-wider"
                  >
                    ACTIVE PLAN
                  </span>
                </div>

                <div class="text-xs text-slate-500 space-y-0.5">
                  <div>Periode: <strong class="text-slate-700">{{ tx.period }}</strong></div>
                  <div>Waktu Transaksi: {{ tx.date }} • Metode: {{ tx.method }}</div>
                  <div class="font-mono text-[11px] text-slate-400">Order ID: {{ tx.orderId }}</div>
                </div>
              </div>

              <!-- Nilai Transaksi & Tombol Download Receipt Kanan -->
              <div class="flex md:flex-col items-center md:items-end justify-between md:justify-center gap-3 pt-3 md:pt-0 border-t md:border-t-0 border-slate-100">
                <div class="text-right">
                  <span class="text-[11px] text-slate-400 block">Total Pembayaran</span>
                  <span class="text-lg font-black text-slate-900">{{ tx.amount }}</span>
                </div>

                <button 
                  type="button"
                  @click="openReceiptModal(tx)"
                  class="px-3.5 py-1.5 bg-white hover:bg-slate-50 text-slate-700 border border-slate-200 rounded-xl text-xs font-semibold transition-all shadow-2xs flex items-center gap-1.5 cursor-pointer"
                >
                  <span>📄</span>
                  <span>Download Receipt</span>
                </button>
              </div>
            </div>
          </div>

        </div>

      </main>
    </div>

    <!-- ================= KWITANSI ELEKTRONIK MODAL (RECEIPT MODAL) ================= -->
    <Teleport to="body">
      <div 
        v-if="isReceiptModalOpen && selectedReceipt"
        class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/40 backdrop-blur-xs select-none"
        @click.self="closeReceiptModal"
      >
        <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl relative space-y-4 text-slate-800 max-h-[90vh] overflow-y-auto">
          <button 
            type="button"
            @click="closeReceiptModal" 
            class="absolute top-5 right-5 text-slate-400 hover:text-slate-700 text-sm p-1 rounded-lg cursor-pointer"
          >
            ✕
          </button>

          <!-- Header Kwitansi -->
          <div class="flex items-start justify-between border-b pb-4">
            <div>
              <h2 class="text-xl font-bold text-orange-600">Subscription Receipt</h2>
              <div class="text-xs text-slate-500 mt-1 space-y-0.5">
                <div>Project: <strong>Testing SA (TSA)</strong></div>
                <div>Subscription ID: <strong>{{ selectedReceipt.id }}</strong></div>
                <div>Order ID: <strong>{{ selectedReceipt.orderId }}</strong></div>
                <div>Date: <strong>{{ selectedReceipt.date }}</strong></div>
                <div>Payment Method: <strong>{{ selectedReceipt.method }}</strong></div>
              </div>
            </div>
            <div class="text-right text-xs">
              <span class="text-slate-400">Bill to:</span>
              <div class="font-bold text-orange-600">Testing SA</div>
              <div class="text-slate-500">Jogja</div>
            </div>
          </div>

          <!-- Table Items -->
          <table class="w-full text-xs">
            <thead>
              <tr class="bg-orange-500 text-white">
                <th class="p-2 text-left rounded-l-lg">Description</th>
                <th class="p-2 text-center">Duration</th>
                <th class="p-2 text-right rounded-r-lg">Amount</th>
              </tr>
            </thead>
            <tbody>
              <tr class="border-b">
                <td class="p-2.5 font-medium">{{ selectedReceipt.name }} Subscription</td>
                <td class="p-2.5 text-center">12 months</td>
                <td class="p-2.5 text-right font-bold">{{ selectedReceipt.amount }}</td>
              </tr>
              <tr class="font-bold text-slate-900">
                <td colspan="2" class="p-2.5 text-right">Total Paid:</td>
                <td class="p-2.5 text-right text-orange-600">{{ selectedReceipt.amount }}</td>
              </tr>
            </tbody>
          </table>

          <!-- Subscription Details Box Hijau -->
          <div class="bg-emerald-50 border border-emerald-300 rounded-xl p-3 text-xs text-emerald-950 space-y-1">
            <div class="font-bold text-emerald-800">■ Subscription Details</div>
            <div>Plan: {{ selectedReceipt.name }}</div>
            <div>Duration: 12 months</div>
            <div>Period: {{ selectedReceipt.period }}</div>
            <div class="font-bold text-emerald-700">Your subscription is now active!</div>
          </div>

          <!-- Footer Actions -->
          <div class="flex items-center justify-between pt-2">
            <span class="text-[11px] text-slate-400">This receipt was generated electronically.</span>
            <div class="flex gap-2">
              <button 
                type="button"
                @click="closeReceiptModal" 
                class="px-4 py-1.5 bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-bold rounded-lg cursor-pointer"
              >
                Tutup
              </button>
              <button 
                type="button"
                @click="printReceipt" 
                class="px-4 py-1.5 bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold rounded-lg shadow-2xs cursor-pointer flex items-center gap-1"
              >
                🖨️ Cetak / Simpan PDF
              </button>
            </div>
          </div>
        </div>
      </div>
    </Teleport>

    <!-- ================= CHECKOUT / UPGRADE MODAL ================= -->
    <Teleport to="body">
      <div 
        v-if="isCheckoutModalOpen && selectedPlanToBuy"
        class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/40 backdrop-blur-xs select-none"
        @click.self="closeCheckoutModal"
      >
        <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-2xl relative space-y-4 text-slate-800">
          <button 
            type="button"
            @click="closeCheckoutModal" 
            class="absolute top-5 right-5 text-slate-400 hover:text-slate-700 text-sm p-1 rounded-lg cursor-pointer"
          >
            ✕
          </button>

          <div class="flex items-center gap-3">
            <div class="w-12 h-12 rounded-xl bg-sky-50 text-sky-600 flex items-center justify-center text-2xl font-bold border border-sky-200">
              {{ selectedPlanToBuy.icon }}
            </div>
            <div>
              <span class="text-[10px] font-extrabold uppercase px-2 py-0.5 rounded-full bg-sky-100 text-sky-800">
                {{ selectedPlanToBuy.categoryLabel }}
              </span>
              <h3 class="text-base font-bold text-slate-900 mt-0.5">{{ selectedPlanToBuy.name }}</h3>
            </div>
          </div>

          <div class="p-3 bg-slate-50 border border-slate-200 rounded-xl space-y-2 text-xs">
            <div class="flex justify-between">
              <span class="text-slate-500">Durasi Langganan:</span>
              <strong class="font-bold">1 Bulan (Perpanjangan Otomatis)</strong>
            </div>
            <div class="flex justify-between">
              <span class="text-slate-500">Kapasitas:</span>
              <strong>{{ selectedPlanToBuy.capacity }}</strong>
            </div>
            <div class="flex justify-between border-t border-slate-200 pt-2 text-sm font-extrabold text-slate-900">
              <span>Total Tagihan:</span>
              <span class="text-sky-600">{{ selectedPlanToBuy.priceFormatted }}</span>
            </div>
          </div>

          <div class="space-y-1.5 text-xs">
            <label class="font-bold text-slate-700 block">Metode Pembayaran:</label>
            <div class="grid grid-cols-3 gap-2">
              <div class="p-2 border-2 border-sky-500 bg-sky-50/50 rounded-xl text-center cursor-pointer font-bold text-sky-800">
                QRIS
              </div>
              <div class="p-2 border border-slate-200 hover:bg-slate-50 rounded-xl text-center cursor-pointer text-slate-600">
                Virtual Account
              </div>
              <div class="p-2 border border-slate-200 hover:bg-slate-50 rounded-xl text-center cursor-pointer text-slate-600">
                Koin TING
              </div>
            </div>
          </div>

          <div class="flex gap-2 pt-2">
            <button 
              type="button"
              @click="closeCheckoutModal" 
              class="w-1/3 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold rounded-xl transition-all cursor-pointer"
            >
              Batal
            </button>
            <button 
              type="button"
              @click="processPayment" 
              class="w-2/3 py-2 bg-sky-600 hover:bg-sky-700 text-white text-xs font-bold rounded-xl shadow-xs transition-all cursor-pointer"
            >
              Bayar Sekarang
            </button>
          </div>
        </div>
      </div>
    </Teleport>

    <!-- ================= TOAST NOTIFICATION ================= -->
    <Teleport to="body">
      <div 
        v-if="isToastVisible" 
        class="fixed bottom-6 left-1/2 -translate-x-1/2 z-50 bg-slate-900/90 text-white text-xs px-4 py-2.5 rounded-xl shadow-xl flex items-center gap-2 backdrop-blur-xs"
      >
        <span>✨</span>
        <span>{{ toastMessage }}</span>
      </div>
    </Teleport>

  </div>
</template>
