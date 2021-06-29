<template>
  <div class="popup">
    <div class="popup__wrapper">
      <div class="popup__header">
        <span>Добавление пользователя</span>
        <button-component @click.native="closePopup">x</button-component>
      </div>
      <hr>
      <div class="popup__content">
        <slot name="content"/>
      </div>
      <hr>
      <div class="popup__footer">
        <slot name="footer" />
      </div>
    </div>
  </div>
</template>

<script>
import ButtonComponent from './Bbutton.vue'

export default {
  name: 'Modal',
  components: {
    ButtonComponent,
  },

  methods: {
    closePopup() {
      this.$emit('closePopup');
    },
  },
}
</script>

<style>
.popup {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  background: #334e4e66;
  right: 0;
  left: 0;
  top: 0;
  bottom: 0;
}

.popup__wrapper {
  width: 350px;
  height: auto;
  padding: 16px;
  position: fixed;
  top: 80px;
  border: 1px solid black;
  background: #ffffff;
  box-shadow: 0 0 17px 0 #e7e7e7;

}
.popup__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.popup__content {
  padding: 0 30px;
}

.popup__form {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
}

.popup__block-data {
  display: flex;
  justify-content: space-between;
  margin: 5px 0;
}

.popup__close {
  border: none;
  background: none;
}

.popup__choose-boss {
  cursor: pointer;
}
</style>
