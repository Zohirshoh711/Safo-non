<template>
  <div class="login-container">
    <!-- Left side - Login Form -->
    <div class="login-form-section">
      <div class="form-wrapper">
        <!-- Logo -->
        <div class="logo-section">
          <div class="logo">
            <img src="../assets/Group 4.png" alt="">
            
          </div>
        </div>

        <!-- Welcome Text -->
        <div class="welcome-section">
          <h1 class="welcome-title">Xush kelibsiz</h1>
          <p class="welcome-subtitle">Iltimos login va parolingizni kiriting</p>
        </div>

        <!-- Login Form -->
        <div class="form-card">
          <form @submit.prevent="handleLogin" class="login-form">
            <div class="form-fields">
              <div class="field-group">
                <div class="input-wrapper">
                  <svg class="input-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
                    <circle cx="12" cy="7" r="4"></circle>
                  </svg>
                  <input
                    id="username"
                    type="text"
                    v-model="username"
                    class="form-input"
                    placeholder="Ismingizni kiriting"
                    required
                  />
                </div>
              </div>

              <div class="field-group">
                <div class="input-wrapper">
                  <svg class="input-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                    <circle cx="12" cy="16" r="1"></circle>
                    <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                  </svg>
                  <input
                    id="password"
                    :type="showPassword ? 'text' : 'password'"
                    v-model="password"
                    class="form-input password-input"
                    placeholder="Parolingizni kiriting"
                    required
                  />
                  <button 
                    type="button" 
                    @click="togglePassword" 
                    class="password-toggle"
                  >
                    <svg v-if="!showPassword" viewBox="0 0 24 24" fill="none" stroke="currentColor">
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
            </div>

            <button type="submit" :disabled="isLoading" class="submit-button">
              {{ isLoading ? 'Kirish...' : 'Sahifaga kirish' }}
            </button>
          </form>
        </div>
      </div>
    </div>

    <!-- Right side - Background -->
    <div class="background-section">
      <div class="background-content">
        <img src="../assets/sAFO-NON.svg" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      username: '',
      password: '',
      showPassword: false,
      isLoading: false
    }
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword
    },
    async handleLogin() {
      this.isLoading = true

      await new Promise(resolve => setTimeout(resolve, 1000))

      // Check admin credentials
      if (this.username === 'admin' && this.password === '1234') {
        localStorage.setItem('isLoggedIn', 'true')
        localStorage.setItem('username', this.username)
        localStorage.setItem('userRole', 'admin')
        
        this.$emit('login-success', {
          username: this.username,
          role: 'admin'
        })
        this.resetForm()
        return
      }

      // Check manager credentials
      const managers = JSON.parse(localStorage.getItem('managers') || '[]')
      const manager = managers.find(m => m.username === this.username && m.password === this.password)
      
      if (manager) {
        localStorage.setItem('isLoggedIn', 'true')
        localStorage.setItem('username', this.username)
        localStorage.setItem('userRole', 'manager')
        localStorage.setItem('managerId', manager.id)
        
        this.$emit('login-success', {
          username: this.username,
          role: 'manager'
        })
        this.resetForm()
        return
      }

      this.$emit('login-error', "Login yoki parol noto'g'ri!")
      this.isLoading = false
    },
    resetForm() {
      this.username = ''
      this.password = ''
      this.showPassword = false
      this.isLoading = false
    }
  }
}
</script>

<style >
.login-container {
  min-height: 100vh;
  display: flex;
}

.login-form-section {
  width: 900px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 32px;
  background-color: white;
}

.form-wrapper {
  width: 600px;
  display: flex;
  flex-direction: column;
  padding-right: 60px;
  gap: 32px;
}

.logo {
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 80px;
  background-color: #6366f1;
  border-radius: 16px;
  margin-bottom: 10px;
}

.logo-sub {
  font-size: 12px;
}

.welcome-section {
  margin-top: 30px;
  margin-bottom: 16px;
}

.welcome-title {
  font-size: 65px;
  font-weight: 500;
  color: #111827;
  margin-bottom: 8px;
}

.welcome-subtitle {
  color: #6b7280;
  font-size: 20px;
}

.form-card {
  background: white;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.form-fields {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.field-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.field-label {
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

.form-input {
  width: 100%;
  height: 48px;
  padding-left: 40px;
  padding-right: 16px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 16px;
  transition: all 0.2s;
  box-sizing: border-box;
}

.password-input {
  padding-right: 48px;
}

.form-input:focus {
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
  margin-top: 50px;
}

.submit-button:hover:not(:disabled) {
  background-color: #5b21b6;
}

.submit-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.background-section {
  width: 800px;
  height: 100vh;
  padding: 20px 20px;
}

.background-content {
  width: 100%;
  height: 100%;
  background-color: #5565FF;
  border: none;
  border-radius: 25px;
  display: flex;
  align-items: center;
  justify-content: center;

}


@media (max-width: 1024px) {
  .background-section {
    display: none;
  }
}

@media (max-width: 768px) {
  .login-form-section {
    padding: 16px;
  }
}
</style>