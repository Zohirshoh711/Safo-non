<template>
  <div class="notification" :class="{ 'notification-show': true, [`notification-${type}`]: true }">
    <div class="notification-content">
      <svg class="notification-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
        <circle v-if="type === 'error'" cx="12" cy="12" r="10"></circle>
        <line v-if="type === 'error'" x1="12" y1="8" x2="12" y2="12"></line>
        <line v-if="type === 'error'" x1="12" y1="16" x2="12.01" y2="16"></line>
        
        <path v-if="type === 'success'" d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
        <polyline v-if="type === 'success'" points="22,4 12,14.01 9,11.01"></polyline>
      </svg>
      <span>{{ message }}</span>
      <button @click="$emit('close')" class="notification-close">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Notification',
  props: {
    message: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'error',
      validator: value => ['error', 'success', 'warning', 'info'].includes(value)
    }
  }
}
</script>

<style scoped>
.notification {
  position: fixed;
  top: -100px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 9999;
  transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.notification-show {
  top: 20px;
}

.notification-content {
  display: flex;
  align-items: center;
  gap: 12px;
  color: white;
  padding: 16px 24px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  min-width: 300px;
}

.notification-error .notification-content {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
}

.notification-success .notification-content {
  background: linear-gradient(135deg, #10b981, #059669);
}

.notification-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.notification-close {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  padding: 4px;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.notification-close:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.notification-close svg {
  width: 16px;
  height: 16px;
}
</style>