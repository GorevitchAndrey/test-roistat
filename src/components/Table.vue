<template>
  <div class="container">
    <div class="table">
      <button-component
          class="btn-open-modal"
          @click.native="showModal"
      >
        Добавить пользователя
      </button-component>
      <table border="1" cellspacing="0" cellpadding="15" width="100%">
        <tr class="table__head">
          <th @click="sortByName">Имя</th>
          <th @click="sortByPhone">Телефон</th>
        </tr>
          <tr
            class="table__body"
            v-for="(user, index) in users"
            :key="index"
          >
            <td
              class="table__name-user"
              @click="hideShow = !hideShow"
            >
              <div class="table__name-boss">{{ user.name }}</div>
              <div
                class="table__new-user subordinate" 
                v-for="(child, index) in user.newUser"
                :key="index"
                v-show="hideShow"
              >
                +{{ child.name }}
              </div>
            </td>
            <td class="table__name-user">
              <div class="table__phone-boss">{{ user.phone }}</div>
              <div
                class="table__new-user"
                v-for="(child, index) in user.newUser"
                :key="index"
                v-show="hideShow"
              >
                {{ child.phone }}
              </div>
            </td>
          </tr>
      </table>
    </div>

    <modal
        v-if="isModalVisible"
        @closePopup="closeModal"
    >
      <template v-slot:content>
        <div class="popup__form">
          <div class="popup__block-data">
            <label>Имя</label>
            <input type="text" v-model.trim="newName">
          </div>
          <div class="popup__block-data">
            <label>Телефон</label>
            <input type="text" v-model.trim="newPhone">
          </div>
          <div class="popup__block-data">
            <label>Начальник</label>
            <select
                class="popup__choose-boss"
                @change="getUserIndex($event.target.selectedIndex)"
            >
              <option
                  v-for="(user, index) in users"
                  :value="user.name"
                  :key="index"
              >
                {{ user.name }}
              </option>
            </select>
          </div>
        </div>
      </template>
      <template v-slot:footer>
        <button-component @click.native="addNewUser">
          Сохранить
        </button-component>
      </template>
    </modal>
  </div>
</template>

<script>
import ButtonComponent from './Bbutton.vue';
import Modal from './Modal.vue';

export default {
  name: 'Table',

  components: {
    ButtonComponent,
    Modal,
  },

  data() {
    return {
      users: [
        {
          name: 'Петр',
          phone: '+7 945 123 16 95'
        },
        {
          name: 'Алексей',
          phone: '+7 915 123 42 79'
        },
        {
          name: 'Анна',
          phone: '+7 915 123 42 77'
        },
        {
          name: 'Марина',
          phone: '+7 915 747 21 42',
        },
        {
          name: 'Борис',
          phone: '+7 964 333 24 42'
        },
        {
          name: 'Андрей',
          phone: '+7 964 111 33 53'
        },
        {
          name: 'Анастсия',
          phone: '+7 977 976 21 26'
        },
      ],
      isModalVisible: false,
      newName: null,
      newPhone: null,
      selectedIndex: 0,
      hideShow: true,
    }
  },

  methods: {
    showModal() {
      this.isModalVisible = true;
    },

    closeModal() {
      this.isModalVisible = false;
    },

    getUserIndex(selectedIndex){
      this.selectedIndex = selectedIndex;
    },

    addNewUser() {
      let newUser = {
        name: this.newName,
        phone: this.newPhone,
      };
      let allUser = this.users;
      let currentBossIndex = this.selectedIndex;

      if (newUser.name && newUser.phone !== '') {

        allUser.forEach((item, index) => {
          if (currentBossIndex === index) {
            item.newUser = [newUser];
            item.name = `+${item.name}`
          }
        })
      }

      this.newName = null;
      this.newPhone = null;

      localStorage.setItem('users', JSON.stringify(allUser))
      this.closeModal();
    },

    sortByName() {
      this.users.sort((a, b) => a.name.localeCompare(b.name));
    },

    sortByPhone() {
      this.users.sort((a, b) => a.phone.localeCompare(b.phone));
    },
  },

  created() {
    const data = localStorage.getItem('users');
    if(data) {
      this.users = JSON.parse(data);
    }
  },
}
</script>

<style>
.table {
  max-width: 500px;
  margin: 0 auto;
}

.table__head {
  cursor: pointer;
}

.table__name-user {
  padding: 0;
}

.table__name-boss {
  display: flex;
  padding-left: 10px;
}

.btn-open-modal {
  margin: 30px 0;
}

.table__new-user {
  border-top: 2px solid #656565;
}

.subordinate {
  display: flex;
  padding-left: 25px;
}

</style>
