<template>
    <div class="warehouse-section">
      <h2 class="section-title">Omborxona hisobot</h2>
      <div class="warehouse-grid">
        <div v-for="report in reports" :key="report.id" class="warehouse-card">
          <div class="warehouse-content">
            <div class="warehouse-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path :d="report.iconPath"></path>
              </svg>
            </div>
            <div class="warehouse-info">
              <div class="warehouse-label">{{ report.label }}</div>
              <div class="warehouse-value">{{ report.value }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'WarehouseReport',
    props: {
      reports: {
        type: Array,
        required: true
      }
    }
  }
  </script>
  
  <style scoped>
  .warehouse-section {
    margin-top: 32px;
  }
  
  .section-title {
    font-size: 24px;
    font-weight: bold;
    color: #1e293b;
    margin-bottom: 24px;
  }
  
  .warehouse-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 24px;
  }
  
  .warehouse-card {
    background: linear-gradient(135deg, #6366f1, #8b5cf6);
    border-radius: 16px;
    padding: 24px;
    color: white;
    transition: transform 0.2s;
  }
  
  .warehouse-card:hover {
    transform: translateY(-2px);
  }
  
  .warehouse-content {
    display: flex;
    align-items: center;
    gap: 16px;
  }
  
  .warehouse-icon {
    width: 32px;
    height: 32px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  
  .warehouse-icon svg {
    width: 18px;
    height: 18px;
  }
  
  .warehouse-info {
    flex: 1;
  }
  
  .warehouse-label {
    font-size: 14px;
    opacity: 0.9;
    margin-bottom: 4px;
  }
  
  .warehouse-value {
    font-size: 28px;
    font-weight: bold;
  }
  
  @media (max-width: 1024px) {
    .warehouse-grid {
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    }
  }
  
  @media (max-width: 768px) {
    .warehouse-grid {
      grid-template-columns: 1fr;
    }
  }
  </style>