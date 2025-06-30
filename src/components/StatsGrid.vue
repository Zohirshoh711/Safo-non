<template>
    <div class="stats-grid">
      <div v-for="stat in stats" :key="stat.id" class="stat-card">
        <div class="stat-content">
          <div class="stat-icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path :d="stat.iconPath"></path>
            </svg>
          </div>
          <div class="stat-info">
            <div class="stat-label">{{ stat.label }}</div>
            <div class="stat-value">{{ stat.value }}</div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'StatsGrid',
    props: {
      stats: {
        type: Array,
        required: true
      }
    }
  }
  </script>
  
  <style scoped>
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 24px;
    margin-bottom: 48px;
  }
  
  .stat-card {
    background: linear-gradient(135deg, #6366f1, #8b5cf6);
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
  
  @media (max-width: 1024px) {
    .stats-grid {
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    }
  }
  
  @media (max-width: 768px) {
    .stats-grid {
      grid-template-columns: 1fr;
    }
  }
  </style>