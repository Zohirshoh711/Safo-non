<template>
  <div class="dashboard-container">
    <!-- Sidebar -->
    <Sidebar
      :menu-items="menuItems"
      :active-menu="'dashboard'"
      :username="username"
      :user-role="userRole"
      @logout="handleLogout"
      @navigate="$emit('navigate', $event)"
    />

    <!-- Main Content -->
    <main class="main-content">
      <div class="content-header">
        <h1 class="page-title">Dashboard</h1>
      </div>

      <!-- Stats Grid -->
      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-content">
            <div class="stat-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path
                  d="M12 2v20M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"
                ></path>
              </svg>
            </div>
            <div class="stat-info">
              <div class="stat-label">Jami Sotuvlar</div>
              <div class="stat-value">₽2,345,000</div>
            </div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-content">
            <div class="stat-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
              </svg>
            </div>
            <div class="stat-info">
              <div class="stat-label">Managerlar</div>
              <div class="stat-value">{{ managersCount }}</div>
            </div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-content">
            <div class="stat-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path
                  d="M9 7h6l2 9H7l2-9zM1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"
                ></path>
              </svg>
            </div>
            <div class="stat-info">
              <div class="stat-label">Buyurtmalar</div>
              <div class="stat-value">856</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Recent Managers -->
      <div class="managers-section" v-if="userRole === 'admin'">
        <h2 class="section-title">So'nggi qo'shilgan managerlar</h2>
        <div class="managers-list">
          <div
            v-for="manager in recentManagers"
            :key="manager.id"
            class="manager-item"
          >
            <div class="manager-info">
              <div class="manager-name">{{ manager.name }}</div>
              <div class="manager-date">
                {{ formatDate(manager.createdAt) }}
              </div>
            </div>
            <div class="manager-status">
              <span class="status-badge">Faol</span>
            </div>
          </div>
          <div v-if="recentManagers.length === 0" class="no-managers">
            <p>Hozircha managerlar yo'q</p>
          </div>
        </div>
      </div>

      <!-- Manager Dashboard -->
      <div class="manager-dashboard" v-if="userRole === 'manager'">
        <h2 class="section-title">Sizning hisobotingiz</h2>
        <div class="manager-stats">
          <div class="manager-stat-card">
            <h3>Bugungi sotuvlar</h3>
            <p class="stat-number">₽125,000</p>
          </div>
          <div class="manager-stat-card">
            <h3>Haftalik sotuvlar</h3>
            <p class="stat-number">₽850,000</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Sidebar from "../components/Sidebar.vue";

export default {
  name: "Dashboard",
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
      menuItems: [
        {
          id: "dashboard",
          name: "Dashboard",
          iconPath: "M3 10a2 2 0 0 1 .709-1.528l7-5.999a2 2 0 0 1 2.582 0l7 5.999A2 2 0 0 1 21 10v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z",
          view: "dashboard",
        },
        {
          id: "managers",
          name: "Managerlar",
          iconPath: "M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2",
          view: "managers",
          adminOnly: true,
        },
        {
          id: "novvoylar",
          name: "Novvoylar",
          iconPath:
            "M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z",
          view: "novvoylar",
        },
        {
          id: "yetkazuvchilar",
          name: "Yetkazuvchilar",
          iconPath:
            "M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z",
          view: "yetkazuvchilar",
        },
        {
          id: "dokonlar",
          name: "Dokonlar",
          iconPath:
            "M9 22C9.55228 22 10 21.5523 10 21C10 20.4477 9.55228 20 9 20C8.44772 20 8 20.4477 8 21C8 21.5523 8.44772 22 9 22ZM20 22C20.5523 22 21 21.5523 21 21C21 20.4477 20.5523 20 20 20C19.4477 20 19 20.4477 19 21C19 21.5523 19.4477 22 20 22ZM1 1H5L7.68 14.39C7.77144 14.8504 8.02191 15.264 8.38755 15.5583C8.75318 15.8526 9.2107 16.009 9.68 16H19.4C19.8693 16.009 20.3268 15.8526 20.6925 15.5583C21.0581 15.264 21.3086 14.8504 21.4 14.39L23 6H6",
          view: "dokonlar",
        },
        {
        id: 'omborxona',
        name: 'Omborxona',
        iconPath: 'M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2H5a2 2 0 00-2 2z M3 7l9-4 9 4',
        view: 'omborxona'
      },
      {
        id: 'chiqimlar',
        name: 'Chiqimlar',
        iconPath: 'M16 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2 M12.5 7a4 4 0 11-8 0 4 4 0 018 0z M16 11l2 2 4-4',
        view: 'chiqimlar'
      },
      {
          id: 'non-turlari',
          name: 'Non turlari',
          iconPath: 'M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z',
          view: 'non-turlari'
        }
      ],
    };
  },
  computed: {
    managersCount() {
      const managers = JSON.parse(localStorage.getItem("managers") || "[]");
      return managers.length;
    },
    recentManagers() {
      const managers = JSON.parse(localStorage.getItem("managers") || "[]");
      return managers.slice(-3).reverse();
    },
  },
  methods: {
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
  },
};
</script>

<style scoped>
.dashboard-container {
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

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 24px;
  margin-bottom: 48px;
}

.stat-card {
  background: #6598ff;
  border-radius: 16px;
  padding: 24px;
  color: white;
  transition: transform 0.2s;
}

.stat-card:hover {
  transform: translateY(-2px);
}

.stat-content {
  display: flex;
  align-items: center;
  gap: 16px;
}

.stat-icon {
  width: 32px;
  height: 32px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.stat-icon svg {
  width: 18px;
  height: 18px;
}

.stat-info {
  flex: 1;
}

.stat-label {
  font-size: 14px;
  opacity: 0.9;
  margin-bottom: 4px;
}

.stat-value {
  font-size: 32px;
  font-weight: bold;
}

.managers-section,
.manager-dashboard {
  margin-top: 32px;
}

.section-title {
  font-size: 24px;
  font-weight: bold;
  color: #1e293b;
  margin-bottom: 24px;
}

.managers-list {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.manager-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 0;
  border-bottom: 1px solid #e2e8f0;
}

.manager-item:last-child {
  border-bottom: none;
}

.manager-name {
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 4px;
}

.manager-date {
  font-size: 14px;
  color: #64748b;
}

.status-badge {
  background-color: #10b981;
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
}

.no-managers {
  text-align: center;
  color: #64748b;
  padding: 32px;
}

.manager-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 24px;
}

.manager-stat-card {
  background: white;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.manager-stat-card h3 {
  color: #64748b;
  font-size: 14px;
  margin-bottom: 8px;
}

.stat-number {
  font-size: 28px;
  font-weight: bold;
  color: #1e293b;
}

@media (max-width: 768px) {
  .dashboard-container {
    flex-direction: column;
  }

  .main-content {
    padding: 16px;
  }

  .stats-grid {
    grid-template-columns: 1fr;
  }
}
</style>
