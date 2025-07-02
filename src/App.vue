<template>
  <div id="app">
    <!-- Error Notification -->
    <div v-if="notification.show" class="notification" :class="notification.type">
      {{ notification.message }}
    </div>

    <!-- Login Page -->
    <LoginPage 
      v-if="currentView === 'login'"
      @login-success="handleLoginSuccess"
      @login-error="showNotification"
    />

    <!-- Dashboard -->
    <Dashboard 
      v-else-if="currentView === 'dashboard'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Managers -->
    <Managers 
      v-else-if="currentView === 'managers'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Novvoylar -->
    <Novvoylar 
      v-else-if="currentView === 'novvoylar'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Dokonlar -->
    <Dokonlar 
      v-else-if="currentView === 'dokonlar'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Yetkazuvchilar -->
    <Yetkazuvchilar 
      v-else-if="currentView === 'yetkazuvchilar'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Omborxona -->
    <Omborxona 
      v-else-if="currentView === 'omborxona'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Chiqimlar -->
    <Chiqimlar 
      v-else-if="currentView === 'chiqimlar'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />

    <!-- Non Turlari -->
    <NonTurlari 
      v-else-if="currentView === 'nonturlari'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />
    <!-- Sozlamalar -->
    <Sozlamalar 
      v-else-if="currentView === 'sozlamalar'"
      :username="currentUser.username"
      :user-role="currentUser.role"
      @logout="handleLogout"
      @navigate="navigateTo"
      @show-notification="showNotification"
    />
  </div>
</template>

<script>
import LoginPage from './views/LoginPage.vue'
import Dashboard from './views/Dashboard.vue'
import Managers from './views/Managers.vue'
import Novvoylar from './views/Novvoylar.vue'
import Dokonlar from './views/Dokonlar.vue'
import Yetkazuvchilar from './views/Yetkazuvchilar.vue'
import Omborxona from './views/Omborxona.vue'
import Chiqimlar from './views/Chiqimlar.vue'
import NonTurlari from './views/NonTurlari.vue'
import Sozlamalar from './views/Sozlamalar.vue'

export default {
  name: 'App',
  components: {
    LoginPage,
    Dashboard,
    Managers,
    Novvoylar,
    Dokonlar,
    Yetkazuvchilar,
    Omborxona,
    Chiqimlar,
    NonTurlari,
    Sozlamalar
  },
  data() {
    return {
      currentView: 'login',
      currentUser: {
        username: '',
        role: ''
      },
      notification: {
        show: false,
        message: '',
        type: 'success'
      }
    }
  },
  mounted() {
    this.checkLoginStatus()
    console.log('App mounted, current view:', this.currentView)
  },
  methods: {
    checkLoginStatus() {
      const isLoggedIn = localStorage.getItem('isLoggedIn')
      const username = localStorage.getItem('username')
      const userRole = localStorage.getItem('userRole')
      
      if (isLoggedIn === 'true' && username && userRole) {
        this.currentUser = {
          username: username,
          role: userRole
        }
        this.currentView = 'dashboard'
      }
    },
    handleLoginSuccess(userData) {
      this.currentUser = userData
      this.currentView = 'dashboard'
    },
    handleLogout() {
      this.currentUser = { username: '', role: '' }
      this.currentView = 'login'
    },
    navigateTo(view) {
      console.log('Navigating to:', view)
      this.currentView = view
    },
    showNotification(data) {
      if (typeof data === 'string') {
        this.notification = {
          show: true,
          message: data,
          type: 'error'
        }
      } else {
        this.notification = {
          show: true,
          message: data.message,
          type: data.type || 'success'
        }
      }
      
      setTimeout(() => {
        this.notification.show = false
      }, 4000)
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
}

#app {
  min-height: 100vh;
}

.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 16px 24px;
  border-radius: 8px;
  color: white;
  font-weight: 500;
  z-index: 9999;
  animation: slideIn 0.3s ease-out;
  max-width: 400px;
  word-wrap: break-word;
  white-space: pre-line;
}

.notification.success {
  background-color: #10b981;
}

.notification.error {
  background-color: #ef4444;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>
