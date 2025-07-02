<template>
    <div class="non-turlari-container"> 
      
      <!-- Sidebar -->
      <Sidebar
        :active-menu="'non-turlari'"
        :username="username"
        :user-role="userRole"
        @logout="handleLogout"
        @navigate="$emit('navigate', $event)"
      />
  
      <!-- Main Content -->
      <main class="main-content">
        <div class="content-header">
          <h1 class="page-title">Non turlari</h1>
          <button @click="showCreateModal = true" class="create-button">
            Non turi qo'shish
          </button>
        </div>
  
        <!-- Non Turlari Table -->
        <div class="table-container">
          <table class="non-turlari-table">
            <thead>
              <tr>
                <th>№</th>
                <th>Non nomi</th>
                <th>Tan narxi</th>
                <th>Dokon narxi</th>
                <th>Toyxona narxi</th>
                <th>Qop narxi</th>
                <th>Holati</th>
                <th>Sana</th>
                <th>Amallar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(non, index) in nonTurlari" :key="non.id">
                <td>{{ index + 1 }}</td>
                <td>{{ non.name }}</td>
                <td>{{ formatCurrency(non.tanNarxi) }}</td>
                <td>{{ formatCurrency(non.dokonNarxi) }}</td>
                <td>{{ formatCurrency(non.toyxonaNarxi) }}</td>
                <td>{{ formatCurrency(non.qopNarxi) }}</td>
                <td>
                  <span class="status-badge" :class="non.status === 'bor' ? 'status-available' : 'status-unavailable'">
                    {{ non.status === 'bor' ? 'Bor' : 'Yo\'q' }}
                  </span>
                </td>
                <td>{{ formatDate(non.createdAt) }}</td>
                <td>
                  <div class="action-buttons">
                    <button
                      @click="viewNon(non)"
                      class="action-btn view-btn"
                    >
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                        <circle cx="12" cy="12" r="3"></circle>
                      </svg>
                    </button>
                    <button
                      @click="editNon(non)"
                      class="action-btn edit-btn"
                    >
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M12 20h9"></path>
                        <path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"></path>
                      </svg>
                    </button>
                    <button
                      @click="deleteNon(non)"
                      class="action-btn delete-btn"
                    >
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <polyline points="3,6 5,6 21,6"></polyline>
                        <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path>
                      </svg>
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
  
          <div v-if="nonTurlari.length === 0" class="empty-state">
            <p>Hozircha non turlari yo'q</p>
          </div>
        </div>
      </main>
  
      <!-- Create Non Modal -->
      <div v-if="showCreateModal" class="modal-overlay" @click="closeModal">
        <div class="modal-content" @click.stop>
          <div class="modal-header">
            <h2>Non turi qo'shish</h2>
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
  
          <form @submit.prevent="createNon" class="modal-form">
            <div class="form-grid">
              <div class="form-group">
                <label for="nonName">Non nomi</label>
                <input
                  id="nonName"
                  type="text"
                  v-model="newNon.name"
                  placeholder="Non nomini kiriting"
                  required
                />
              </div>
  
              <div class="form-group">
                <label for="nonStatus">Holati</label>
                <select
                  id="nonStatus"
                  v-model="newNon.status"
                  required
                >
                  <option value="">Holatni tanlang</option>
                  <option value="bor">Bor</option>
                  <option value="yoq">Yo'q</option>
                </select>
              </div>
  
              <div class="form-group">
                <label for="tanNarxi">Tan narxi (so'm)</label>
                <input
                  id="tanNarxi"
                  type="number"
                  v-model="newNon.tanNarxi"
                  placeholder="0"
                  min="0"
                  step="100"
                  required
                />
              </div>
  
              <div class="form-group">
                <label for="dokonNarxi">Dokon narxi (so'm)</label>
                <input
                  id="dokonNarxi"
                  type="number"
                  v-model="newNon.dokonNarxi"
                  placeholder="0"
                  min="0"
                  step="100"
                  required
                />
              </div>
  
              <div class="form-group">
                <label for="toyxonaNarxi">Toyxona narxi (so'm)</label>
                <input
                  id="toyxonaNarxi"
                  type="number"
                  v-model="newNon.toyxonaNarxi"
                  placeholder="0"
                  min="0"
                  step="100"
                  required
                />
              </div>
  
              <div class="form-group">
                <label for="qopNarxi">Qop narxi (so'm)</label>
                <input
                  id="qopNarxi"
                  type="number"
                  v-model="newNon.qopNarxi"
                  placeholder="0"
                  min="0"
                  step="100"
                  required
                />
              </div>
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
    name: "NonTurlari",
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
        nonTurlari: [],
        showCreateModal: false,
        isCreating: false,
        newNon: {
          name: "",
          tanNarxi: 0,
          dokonNarxi: 0,
          toyxonaNarxi: 0,
          qopNarxi: 0,
          status: "",
        },
      };
    },
    mounted() {
      console.log('NonTurlari component mounted')
      this.loadNonTurlari();
    },
    methods: {
      loadNonTurlari() {
        this.nonTurlari = JSON.parse(localStorage.getItem("nonTurlari") || "[]");
      },
      async createNon() {
        this.isCreating = true;
  
        await new Promise((resolve) => setTimeout(resolve, 1000));
  
        const newNon = {
          id: Date.now().toString(),
          name: this.newNon.name,
          tanNarxi: Number(this.newNon.tanNarxi),
          dokonNarxi: Number(this.newNon.dokonNarxi),
          toyxonaNarxi: Number(this.newNon.toyxonaNarxi),
          qopNarxi: Number(this.newNon.qopNarxi),
          status: this.newNon.status,
          createdAt: new Date().toISOString(),
        };
  
        this.nonTurlari.push(newNon);
        localStorage.setItem("nonTurlari", JSON.stringify(this.nonTurlari));
  
        this.$emit("show-notification", {
          message: "Non turi muvaffaqiyatli qo'shildi!",
          type: "success",
        });
  
        this.closeModal();
        this.isCreating = false;
      },
      viewNon(non) {
        alert(
          `Non ma'lumotlari:
  Nomi: ${non.name}
  Tan narxi: ${this.formatCurrency(non.tanNarxi)}
  Dokon narxi: ${this.formatCurrency(non.dokonNarxi)}
  Toyxona narxi: ${this.formatCurrency(non.toyxonaNarxi)}
  Qop narxi: ${this.formatCurrency(non.qopNarxi)}
  Holati: ${non.status === 'bor' ? 'Bor' : 'Yo\'q'}
  Qo'shilgan: ${this.formatDate(non.createdAt)}`
        );
      },
      editNon(non) {
        const newName = prompt("Yangi non nomi:", non.name);
        if (newName && newName.trim()) {
          const newTanNarxi = prompt("Yangi tan narxi:", non.tanNarxi);
          if (newTanNarxi !== null) {
            const newDokonNarxi = prompt("Yangi dokon narxi:", non.dokonNarxi);
            if (newDokonNarxi !== null) {
              const newToyxonaNarxi = prompt("Yangi toyxona narxi:", non.toyxonaNarxi);
              if (newToyxonaNarxi !== null) {
                const newQopNarxi = prompt("Yangi qop narxi:", non.qopNarxi);
                if (newQopNarxi !== null) {
                  non.name = newName.trim();
                  non.tanNarxi = Number(newTanNarxi) || 0;
                  non.dokonNarxi = Number(newDokonNarxi) || 0;
                  non.toyxonaNarxi = Number(newToyxonaNarxi) || 0;
                  non.qopNarxi = Number(newQopNarxi) || 0;
                  
                  localStorage.setItem("nonTurlari", JSON.stringify(this.nonTurlari));
  
                  this.$emit("show-notification", {
                    message: "Non ma'lumotlari yangilandi!",
                    type: "success",
                  });
                }
              }
            }
          }
        }
      },
      deleteNon(non) {
        if (confirm(`${non.name} non turini o'chirishni xohlaysizmi?`)) {
          this.nonTurlari = this.nonTurlari.filter((n) => n.id !== non.id);
          localStorage.setItem("nonTurlari", JSON.stringify(this.nonTurlari));
  
          this.$emit("show-notification", {
            message: "Non turi o'chirildi!",
            type: "success",
          });
        }
      },
      closeModal() {
        this.showCreateModal = false;
        this.newNon = { 
          name: "", 
          tanNarxi: 0, 
          dokonNarxi: 0, 
          toyxonaNarxi: 0, 
          qopNarxi: 0, 
          status: "" 
        };
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
  .non-turlari-container {
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
  
  .non-turlari-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .non-turlari-table th {
    background-color: #f8fafc;
    padding: 16px;
    text-align: left;
    font-weight: 600;
    color: #374151;
    border-bottom: 1px solid #e2e8f0;
    font-size: 14px;
  }
  
  .non-turlari-table td {
    padding: 16px;
    border-bottom: 1px solid #e2e8f0;
    color: #1e293b;
    font-size: 14px;
  }
  
  .non-turlari-table tr:last-child td {
    border-bottom: none;
  }
  
  .status-badge {
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 500;
  }
  
  .status-available {
    background-color: #10b981;
    color: white;
  }
  
  .status-unavailable {
    background-color: #ef4444;
    color: white;
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
  
  .action-btn svg {
    width: 16px;
    height: 16px;
  }
  
  .view-btn {
    background-color: #3b82f6;
    color: white;
  }
  
  .view-btn:hover {
    background-color: #2563eb;
  }
  
  .edit-btn {
    background-color: #10b981;
    color: white;
  }
  
  .edit-btn:hover {
    background-color: #059669;
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
    .non-turlari-container {
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
  
    .non-turlari-table {
      font-size: 12px;
    }
  
    .non-turlari-table th,
    .non-turlari-table td {
      padding: 8px 4px;
    }
  }
  </style>
  