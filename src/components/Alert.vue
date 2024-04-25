<template>
  <div class="alert" :class="alertType">
    <p>{{ message }}</p>
    <button class="close-button" @click="closeAlert">&times;</button>
  </div>
</template>

<script>
export default {
  name: 'Alert',
  props: {
    message: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'danger',
      validator: function(value) {
        return ['success','danger'].indexOf(value) !== -1;
      }
    }
  },
  computed: {
    alertType: function() {
      return 'alert-' + this.type;
    }
  },
  methods: {
   alertType() {
      return 'alert-' + this.type;
    },
    closeAlert() {
      this.$emit('close');
    }
  }
};
</script>

<style>
.alert {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border: 1px solid transparent;
  border-radius: 4px;
  margin-bottom: 15px;
}

.alert p {
  margin: 0;
}

.alert button.close-button {
  background-color: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: inherit;
  padding: 0;
}

.alert-success {
  background-color: #d4edda;
  border-color: #c3e6cb;
  color: #155724;
}

.alert-danger {
  background-color: #f8d7da;
  border-color: #f5c6cb;
  color: #721c24;
}
</style>
