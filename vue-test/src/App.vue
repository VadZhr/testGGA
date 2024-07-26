<script>
import User from "./components/User.vue";
import ModalForm from "./components/ModalForm.vue";

export default {
  name: "App",
  components: {
    User,
    ModalForm,
  },
  data() {
    return {
      users: [
        {
          name: "Вадим",
          number: "87777077077",
          id: 1,
          slaves: [
            {
              name: "Денис",
              number: "87777077077",
              id: 2,
              slaves: [
                {
                  name: "Петя",
                  number: "87777077077",
                  id: 3,
                  slaves: [
                    { name: "Дима", number: "87777077077", id: 4, slaves: [] },
                  ],
                },
              ],
            },
          ],
        },
        {
          name: "Артем",
          number: "87777077077",
          id: 9,
          slaves: [],
        },
      ],
      showForm: false,
    };
  },
  mounted(){
    this.getUsersFromLocalStorage()
  },
  methods: {
    addUser() {
      this.showForm = true;
    },
    closeForm() {
      this.showForm = false;
    },
    addNewUser(data) {
      if(!data.idOfBoss){
        this.users.push({name:data.name,number:data.number, id:Date.now(), slaves:[]})
      }
      const h = (users) => {
        return users.map((el) => {
          if (el.id == data.idOfBoss) {
            el.slaves.push({name:data.name,number:data.number, id:Date.now(), slaves:[]});
            return el;
          }
          if (el.slaves.length) {
            el.slaves = h(el.slaves);
          }

          return el;
        });
      };
      this.users=h(this.users)
      localStorage.setItem('users',JSON.stringify(this.users))
    },
    getUsersFromLocalStorage(){
    const dataFromLS=localStorage.getItem('users')
    if(dataFromLS){
      this.users=JSON.parse(dataFromLS)
    }{
      localStorage.setItem('users',JSON.stringify(this.users))
    }
   
  }
  },

};
</script>

<template>
  <div id="app">
    <div>
      <ModalForm
        v-if="showForm"
        @closeForm="closeForm"
        :users="users"
        @addNewUser="addNewUser"
      />
    </div>

    <div>
      <button @click="addUser">Добавить</button>
    </div>

    <div class="table">
      <div class="tableHead">
        <div class="tableName">Имя</div>
        <div class="tableNumber">Телефон</div>
        <div class="divider"></div>
      </div>
      <User
        v-for="(el, index) in users"
        :user="el"
        :count="1"
        :borderTop="'1px solid black'"
      />
    </div>
  </div>
</template>

<style>
#app {
  display: flex;
  flex-direction: column;
}
.table {
  display: flex;
  flex-direction: column;
  width: fit-content;
}
.tableHead {
  display: flex;

  justify-content: space-between;
  border: 1px solid black;
  margin-left: 10px;
  border-bottom: none;
  position: relative;

  /* padding: 10px; */
}
.tableName {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  flex: 1;
}
.tableNumber {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;

  flex: 1;
}
.divider {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 1px;
  background-color: #000;
}
</style>
