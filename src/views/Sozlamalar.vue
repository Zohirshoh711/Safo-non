<template>
  <div class="sozlamalar-container">
    <!-- Sidebar -->
    <Sidebar
      :active-menu="'sozlamalar'"
      :username="username"
      :user-role="userRole"
      @logout="handleLogout"
      @navigate="$emit('navigate', $event)"
    />

    <!-- Main Content -->
    <main class="main-content">
      <div class="content-header">
        <h1 class="page-title">Sozlamalar</h1>
      </div>

      <!-- Settings Cards -->
      <div class="settings-grid">
        <!-- Password Change Card -->
        <div class="settings-card">
          <div class="card-header">
            <h2 class="card-title">Parolni o'zgartirish</h2>
            <p class="card-description">Xavfsizlik uchun parolingizni muntazam o'zgartiring</p>
          </div>

          <form @submit.prevent="changePassword" class="password-form">
            <div class="form-group">
              <label for="currentPassword">Joriy parol</label>
              <div class="input-wrapper">
                <svg class="input-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                  <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                  <circle cx="12" cy="16" r="1"></circle>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                <input
                  id="currentPassword"
                  :type="showCurrentPassword ? 'text' : 'password'"
                  v-model="passwordForm.currentPassword"
                  placeholder="Joriy parolingizni kiriting"
                  required
                />
                <button 
                  type="button" 
                  @click="showCurrentPassword = !showCurrentPassword" 
                  class="password-toggle"
                >
                  <svg v-if="!showCurrentPassword" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                  <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                    <line x1="1" y1="1" x2="23" y2="23"></line>
                  </svg>
                </button>
              </div>
            </div>

            <div class="form-group">
              <label for="newPassword">Yangi parol</label>
              <div class="input-wrapper">
                <svg class="input-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                  <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                  <circle cx="12" cy="16" r="1"></circle>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                <input
                  id="newPassword"
                  :type="showNewPassword ? 'text' : 'password'"
                  v-model="passwordForm.newPassword"
                  placeholder="Yangi parolingizni kiriting"
                  required
                />
                <button 
                  type="button" 
                  @click="showNewPassword = !showNewPassword" 
                  class="password-toggle"
                >
                  <svg v-if="!showNewPassword" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                  <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                    <line x1="1" y1="1" x2="23" y2="23"></line>
                  </svg>
                </button>
              </div>
            </div>

            <div class="form-group">
              <label for="confirmPassword">Yangi parolni tasdiqlang</label>
              <div class="input-wrapper">
                <svg class="input-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                  <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                  <circle cx="12" cy="16" r="1"></circle>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
                <input
                  id="confirmPassword"
                  :type="showConfirmPassword ? 'text' : 'password'"
                  v-model="passwordForm.confirmPassword"
                  placeholder="Yangi parolni qayta kiriting"
                  required
                />
                <button 
                  type="button" 
                  @click="showConfirmPassword = !showConfirmPassword" 
                  class="password-toggle"
                >
                  <svg v-if="!showConfirmPassword" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                  <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                    <line x1="1" y1="1" x2="23" y2="23"></line>
                  </svg>
                </button>
              </div>
            </div>

            <button type="submit" :disabled="isChangingPassword" class="submit-button">
              {{ isChangingPassword ? 'O\'zgartirilmoqda...' : 'Parolni o\'zgartirish' }}
            </button>
          </form>
        </div>

        <!-- User Info Card -->
        <div class="settings-card">
          <div class="card-header">
            <h2 class="card-title">Foydalanuvchi ma'lumotlari</h2>
            <p class="card-description">Sizning hisob ma'lumotlaringiz</p>
          </div>

          <div class="user-info">
            <div class="info-item">
              <span class="info-label">Foydalanuvchi nomi:</span>
              <span class="info-value">{{ username }}</span>
            </div>
            <div class="info-item">
              <span class="info-label">Rol:</span>
              <span class="info-value role-badge" :class="userRole">
                {{ userRole === 'admin' ? 'Administrator' : 'Manager' }}
              </span>
            </div>
            <div class="info-item">
              <span class="info-label">Oxirgi kirish:</span>
              <span class="info-value">{{ getCurrentDate() }}</span>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Sidebar from "../components/Sidebar.vue";

export default {
  name: "Sozlamalar",
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
      passwordForm: {
        currentPassword: "",
        newPassword: "",
        confirmPassword: "",
      },
      showCurrentPassword: false,
      showNewPassword: false,
      showConfirmPassword: false,
      isChangingPassword: false,
    };
  },
  methods: {
    async changePassword() {
      // Validatsiya
      if (this.passwordForm.newPassword !== this.passwordForm.confirmPassword) {
        this.$emit("show-notification", {
          message: "Yangi parollar mos kelmaydi!",
          type: "error",
        });
        return;
      }

      if (this.passwordForm.newPassword.length < 4) {
        this.$emit("show-notification", {
          message: "Yangi parol kamida 4 ta belgidan iborat bo'lishi kerak!",
          type: "error",
        });
        return;
      }

      this.isChangingPassword = true;

      try {
        // Simulate API request
        await new Promise((resolve) => setTimeout(resolve, 1500));

        // Admin uchun parolni tekshirish
        if (this.userRole === 'admin') {
          // Joriy admin parolini localStorage dan olish
          const currentAdminPassword = localStorage.getItem('adminPassword') || '1234';
          
          if (this.passwordForm.currentPassword !== currentAdminPassword) {
            this.$emit("show-notification", {
              message: "Joriy parol noto'g'ri!",
              type: "error",
            });
            this.isChangingPassword = false;
            return;
          }
          
          // Yangi admin parolini localStorage ga saqlash
          localStorage.setItem('adminPassword', this.passwordForm.newPassword);
          console.log('Admin paroli o\'zgartirildi:', this.passwordForm.newPassword);
        } else {
          // Manager uchun parolni tekshirish
          const managers = JSON.parse(localStorage.getItem('managers') || '[]');
          const currentManager = managers.find(m => 
            (m.username === this.username || m.name === this.username)
          );
          
          if (!currentManager || currentManager.password !== this.passwordForm.currentPassword) {
            this.$emit("show-notification", {
              message: "Joriy parol noto'g'ri!",
              type: "error",
            });
            this.isChangingPassword = false;
            return;
          }
          
          // Manager parolini yangilash
          currentManager.password = this.passwordForm.newPassword;
          localStorage.setItem('managers', JSON.stringify(managers));
        }

        this.$emit("show-notification", {
          message: "Parol muvaffaqiyatli o'zgartirildi!",
          type: "success",
        });

        // Formani tozalash
        this.passwordForm = {
          currentPassword: "",
          newPassword: "",
          confirmPassword: "",
        };

      } catch (error) {
        this.$emit("show-notification", {
          message: "Parolni o'zgartirishda xatolik yuz berdi!",
          type: "error",
        });
      }

      this.isChangingPassword = false;
    },
    handleLogout() {
      localStorage.removeItem("isLoggedIn");
      localStorage.removeItem("username");
      localStorage.removeItem("userRole");
      localStorage.removeItem("managerId");
      this.$emit("logout");
    },
    getCurrentDate() {
      return new Date().toLocaleDateString("uz-UZ");
    },
  },
};
</script>

<style scoped>
.sozlamalar-container {
  display: flex;
  min-height: 100vh;
  background-color: #f8fafc;
}

.main-content {
  flex: 1;
  padding: 32px;
  overflow-y: auto;
}

.content-header {
  margin-bottom: 32px;
}

.page-title {
  font-size: 32px;
  font-weight: bold;
  color: #1e293b;
}

.settings-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
}

.settings-card {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  border: 1px solid #e2e8f0;
}

.card-header {
  margin-bottom: 24px;
}

.card-title {
  font-size: 20px;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 8px;
}

.card-description {
  color: #64748b;
  font-size: 14px;
}

.password-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  font-size: 14px;
  font-weight: 500;
  color: #374151;
}

.input-wrapper {
  position: relative;
}

.input-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  color: #9ca3af;
  z-index: 1;
}

.form-group input {
  width: 100%;
  height: 48px;
  padding-left: 40px;
  padding-right: 48px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 16px;
  transition: all 0.2s;
  box-sizing: border-box;
}

.form-group input:focus {
  outline: none;
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

.password-toggle {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  color: #9ca3af;
  transition: color 0.2s;
}

.password-toggle:hover {
  color: #6366f1;
}

.password-toggle svg {
  width: 20px;
  height: 20px;
}

.submit-button {
  width: 100%;
  height: 48px;
  background-color: #6366f1;
  color: white;
  font-weight: 500;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.2s;
  margin-top: 8px;
}

.submit-button:hover:not(:disabled) {
  background-color: #5b21b6;
}

.submit-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.user-info {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.info-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #f1f5f9;
}

.info-item:last-child {
  border-bottom: none;
}

.info-label {
  font-weight: 500;
  color: #64748b;
}

.info-value {
  font-weight: 600;
  color: #1e293b;
}

.role-badge {
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
}

.role-badge.admin {
  background-color: #dc2626;
  color: white;
}

.role-badge.manager {
  background-color: #059669;
  color: white;
}

@media (max-width: 1024px) {
  .settings-grid {
    grid-template-columns: 1fr;
    gap: 24px;
  }
}

@media (max-width: 768px) {
  .sozlamalar-container {
    flex-direction: column;
  }
  
  .main-content {
    padding: 16px;
  }
  
  .settings-card {
    padding: 20px;
  }
}
</style>
