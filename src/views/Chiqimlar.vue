<template>
    <div class="chiqimlar-container">
      <!-- Sidebar -->
      <Sidebar
        :active-menu="'chiqimlar'"
        :username="username"
        :user-role="userRole"
        @logout="handleLogout"
        @navigate="$emit('navigate', $event)"
      />
  
      <!-- Main Content -->
      <main class="main-content">
        <div class="content-header">
          <h1 class="page-title">Chiqimlar</h1>
          <button @click="showCreateModal = true" class="create-button">
            Chiqim qo'shish
          </button>
        </div>
  
        <!-- Chiqimlar Table -->
        <div class="table-container">
          <table class="chiqimlar-table">
            <thead>
              <tr>
                <th>№</th>
                <th>Mahsulot</th>
                <th>Novvoy</th>
                <th>Narxi</th>
                <th>Soni</th>
                <th>Jami</th>
                <th>Sana</th>
                <th>Amallar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(chiqim, index) in chiqimlar" :key="chiqim.id">
                <td>{{ index + 1 }}</td>
                <td>{{ chiqim.productName }}</td>
                <td>{{ chiqim.novvoyName }}</td>
                <td>{{ formatCurrency(chiqim.price) }}</td>
                <td>{{ chiqim.quantity }} dona</td>
                <td class="total-amount">{{ formatCurrency(chiqim.price * chiqim.quantity) }}</td>
                <td>{{ formatDate(chiqim.createdAt) }}</td>
                <td>
                  <div class="action-buttons">
                    <button
                      @click="viewChiqim(chiqim)"
                      class="action-btn view-btn"
                    >
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" width="16" height="16">
                        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                        <circle cx="12" cy="12" r="3"></circle>
                      </svg>
                    </button>
                    <button
                      @click="deleteChiqim(chiqim)"
                      class="action-btn delete-btn"
                    >
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" width="16" height="16">
                        <polyline points="3,6 5,6 21,6"></polyline>
                        <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path>
                      </svg>
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
  
          <div v-if="chiqimlar.length === 0" class="empty-state">
            <p>Hozircha chiqimlar yo'q</p>
          </div>
        </div>
      </main>
  
      <!-- Create Chiqim Modal -->
      <div v-if="showCreateModal" class="modal-overlay" @click="closeModal">
        <div class="modal-content" @click.stop>
          <div class="modal-header">
            <h2>Chiqim qo'shish</h2>
            <button style="width: 50px; height: 40px; all: unset; margin: 0; padding: 0;" @click="closeModal" class="close-button">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="40"
                height="40"
                viewBox="0 0 24 24"
                fill="none"
                stroke="red"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M18 6 6 18" />
                <path d="m6 6 12 12" />
              </svg>
            </button>
          </div>
  
          <form @submit.prevent="createChiqim" class="modal-form">
            <div class="form-grid">
              <div class="form-group">
                <label for="productName">Mahsulot</label>
                <select
                  id="productName"
                  v-model="newChiqim.productName"
                  required
                >
                  <option value="">Mahsulotni tanlang</option>
                  <option v-for="mahsulot in availableProducts" :key="mahsulot.id" :value="mahsulot.name">
                    {{ mahsulot.name }} ({{ mahsulot.quantity }} dona)
                  </option>
                </select>
              </div>
  
              <div class="form-group">
                <label for="novvoyName">Novvoy</label>
                <select
                  id="novvoyName"
                  v-model="newChiqim.novvoyName"
                  required
                >
                  <option value="">Novvoyni tanlang</option>
                  <option v-for="novvoy in novvoylar" :key="novvoy.id" :value="novvoy.name">
                    {{ novvoy.name }}
                  </option>
                </select>
              </div>
  
              <div class="form-group">
                <label for="chiqimPrice">Narxi (so'm)</label>
                <input
                  id="chiqimPrice"
                  type="number"
                  v-model="newChiqim.price"
                  placeholder="0"
                  min="0"
                  step="1000"
                  required
                />
              </div>
  
              <div class="form-group">
                <label for="chiqimQuantity">Soni</label>
                <input
                  id="chiqimQuantity"
                  type="number"
                  v-model="newChiqim.quantity"
                  placeholder="0"
                  min="1"
                  required
                />
              </div>
            </div>
  
            <div class="total-preview" v-if="newChiqim.price && newChiqim.quantity">
              <h3>Jami: {{ formatCurrency(newChiqim.price * newChiqim.quantity) }}</h3>
            </div>
  
            <div class="modal-actions">
              <button type="button" @click="closeModal" class="cancel-button">
                Chiqish
              </button>
              <button type="submit" :disabled="isCreating" class="submit-button1">
                {{ isCreating ? "Qo'shilmoqda..." : "Qo'shish" }}
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import Sidebar from "../components/Sidebar.vue";
  
  export default {
    name: "Chiqimlar",
    components: {
      Sidebar,
    },
    props: {
      username: {
        type: String,
        required: true,
      },
      userRole: {
        type: String,
        required: true,
      },
    },
    data() {
      return {
        chiqimlar: [],
        showCreateModal: false,
        isCreating: false,
        newChiqim: {
          productName: "",
          novvoyName: "",
          price: 0,
          quantity: 1,
        },
        availableProducts: [],
        novvoylar: [],
      };
    },
    mounted() {
      this.loadChiqimlar();
      this.loadAvailableProducts();
      this.loadNovvoylar();
    },
    methods: {
      loadChiqimlar() {
        this.chiqimlar = JSON.parse(localStorage.getItem("chiqimlar") || "[]");
      },
      loadAvailableProducts() {
        const omborxona = JSON.parse(localStorage.getItem("omborxona") || "[]");
        this.availableProducts = omborxona.filter(mahsulot => mahsulot.status === 'bor' && mahsulot.quantity > 0);
      },
      loadNovvoylar() {
        this.novvoylar = JSON.parse(localStorage.getItem("novvoylar") || "[]");
      },
      async createChiqim() {
        this.isCreating = true;
  
        await new Promise((resolve) => setTimeout(resolve, 1000));
  
        const newChiqim = {
          id: Date.now().toString(),
          productName: this.newChiqim.productName,
          novvoyName: this.newChiqim.novvoyName,
          price: Number(this.newChiqim.price),
          quantity: Number(this.newChiqim.quantity),
          createdAt: new Date().toISOString(),
        };
  
        this.chiqimlar.push(newChiqim);
        localStorage.setItem("chiqimlar", JSON.stringify(this.chiqimlar));
  
        // Omborxonadan mahsulot sonini kamaytirish
        this.updateProductQuantity(newChiqim.productName, newChiqim.quantity);
  
        this.$emit("show-notification", {
          message: "Chiqim muvaffaqiyatli qo'shildi!",
          type: "success",
        });
  
        this.closeModal();
        this.isCreating = false;
        this.loadAvailableProducts(); // Yangilash
      },
      updateProductQuantity(productName, usedQuantity) {
        const omborxona = JSON.parse(localStorage.getItem("omborxona") || "[]");
        const product = omborxona.find(p => p.name === productName);
        
        if (product) {
          product.quantity -= usedQuantity;
          if (product.quantity <= 0) {
            product.status = 'yoq';
            product.quantity = 0;
          }
          localStorage.setItem("omborxona", JSON.stringify(omborxona));
        }
      },
      viewChiqim(chiqim) {
        alert(
          `Chiqim ma'lumotlari:
  Mahsulot: ${chiqim.productName}
  Novvoy: ${chiqim.novvoyName}
  Narxi: ${this.formatCurrency(chiqim.price)}
  Soni: ${chiqim.quantity} dona
  Jami: ${this.formatCurrency(chiqim.price * chiqim.quantity)}
  Sana: ${this.formatDate(chiqim.createdAt)}`
        );
      },
      deleteChiqim(chiqim) {
        if (confirm(`Bu chiqimni o'chirishni xohlaysizmi?`)) {
          this.chiqimlar = this.chiqimlar.filter((c) => c.id !== chiqim.id);
          localStorage.setItem("chiqimlar", JSON.stringify(this.chiqimlar));
  
          this.$emit("show-notification", {
            message: "Chiqim o'chirildi!",
            type: "success",
          });
        }
      },
      closeModal() {
        this.showCreateModal = false;
        this.newChiqim = { productName: "", novvoyName: "", price: 0, quantity: 1 };
      },
      handleLogout() {
        localStorage.removeItem("isLoggedIn");
        localStorage.removeItem("username");
        localStorage.removeItem("userRole");
        localStorage.removeItem("managerId");
        this.$emit("logout");
      },
      formatDate(dateString) {
        const date = new Date(dateString);
        return date.toLocaleDateString("uz-UZ");
      },
      formatCurrency(amount) {
        return new Intl.NumberFormat('uz-UZ').format(amount) + " so'm";
      },
    },
  };
  </script>
  
  <style scoped>
  .chiqimlar-container {
    display: flex;
    min-height: 100vh;
    background-color: white;
  }
  
  .main-content {
    flex: 1;
    padding: 32px;
    overflow-y: auto;
  }
  
  .content-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 32px;
  }
  
  .page-title {
    font-size: 32px;
    font-weight: bold;
    color: #1e293b;
  }
  
  .create-button {
    background-color: #6366f1;
    color: white;
    padding: 12px 24px;
    border: none;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  
  .create-button:hover {
    background-color: #5b21b6;
  }
  
  .table-container {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  }
  
  .chiqimlar-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .chiqimlar-table th {
    background-color: #f8fafc;
    padding: 16px;
    text-align: left;
    font-weight: 600;
    color: #374151;
    border-bottom: 1px solid #e2e8f0;
  }
  
  .chiqimlar-table td {
    padding: 16px;
    border-bottom: 1px solid #e2e8f0;
    color: #1e293b;
  }
  
  .chiqimlar-table tr:last-child td {
    border-bottom: none;
  }
  
  .total-amount {
    font-weight: 600;
    color: #059669;
  }
  
  .action-buttons {
    display: flex;
    gap: 8px;
  }
  
  .action-btn {
    width: 32px;
    height: 32px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s;
  }
  
  .view-btn {
    background-color: #3b82f6;
    color: white;
  }
  
  .view-btn:hover {
    background-color: #2563eb;
  }
  
  .delete-btn {
    background-color: #ef4444;
    color: white;
  }
  
  .delete-btn:hover {
    background-color: #dc2626;
  }
  
  .empty-state {
    padding: 48px;
    text-align: center;
    color: #64748b;
  }
  
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: flex-end;
    z-index: 1000;
  }
  
  .modal-content {
    background: white;
    width: 100%;
    max-width: 1450px;
    height: 100%;
    border-radius: 30px 0 0 30px;
    box-shadow: -2px 0 8px rgba(0, 0, 0, 0.15);
    animation: slideInRight 0.6s ease-out;
    overflow-y: auto;
  }
  
  @keyframes slideInRight {
    from {
      transform: translateX(100%);
    }
    to {
      transform: translateX(0);
    }
  }
  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 50px 80px;
    border-bottom: 1px solid #e2e8f0;
  }
  
  .modal-header h2 {
    font-size: 30px;
    font-weight: 600;
    color: #1e293b;
  }
  
  .close-button {
    background: none;
    border: none;
    cursor: pointer;
    color: #64748b;
    transition: color 0.2s;
  }
  
  .close-button:hover {
    color: #1e293b;
  }
  
  .modal-form {
    padding: 80px;
  }
  
  .form-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 50px;
    margin-bottom: 50px;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
  }
  
  .form-group label {
    margin-bottom: 8px;
    font-weight: 500;
    color: #374151;
    font-size: 16px;
  }
  
  .form-group input,
  .form-group select {
    height: 48px;
    padding: 0 16px;
    border: 1px solid #d1d5db;
    border-radius: 8px;
    font-size: 16px;
    transition: border-color 0.2s;
    box-sizing: border-box;
  }
  
  .form-group input:focus,
  .form-group select:focus {
    outline: none;
    border-color: #6366f1;
    box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
  }
  
  .total-preview {
    text-align: center;
    margin: 30px 0;
    padding: 20px;
    background-color: #f0f9ff;
    border-radius: 8px;
    border: 2px solid #0ea5e9;
  }
  
  .total-preview h3 {
    color: #0369a1;
    font-size: 24px;
  }
  
  .modal-actions {
    display: flex;
    align-items: center;
    justify-content: end;
    gap: 30px;
    margin-top: 100px;
  }
  
  .modal-actions button {
    width: 200px;
    height: 50px;
  }
  
  .cancel-button {
    padding: 12px 24px;
    border: 1px solid #ff0000;
    background: rgb(255, 71, 71);
    color: #ffffff;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
    font-size: 16px;
  }
  
  .cancel-button:hover {
    background-color: #ff2020;
  }
  
  .submit-button1 {
    padding: 12px 24px;
    background-color: #6366f1;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: background-color 0.2s;
    font-size: 15px;
  }
  
  .submit-button1:hover:not(:disabled) {
    background-color: #303af5;
  }
  
  .submit-button1:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
  
  @media (max-width: 768px) {
    .chiqimlar-container {
      flex-direction: column;
    }
  
    .main-content {
      padding: 16px;
    }
  
    .content-header {
      flex-direction: column;
      gap: 16px;
      align-items: stretch;
    }
  
    .form-grid {
      grid-template-columns: 1fr;
      gap: 20px;
    }
  
    .chiqimlar-table {
      font-size: 14px;
    }
  
    .chiqimlar-table th,
    .chiqimlar-table td {
      padding: 12px 8px;
    }
  }
  </style>
  