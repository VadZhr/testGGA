<script>
export default {
  name: "ModalForm",
  props: ["users"],
  data() {
    return {
        bosses:[1,2,3,4],
        idOfBoss:0,
        nameError:false,
        numberError:false,
        name:'',
        number:''
    };
  },
  methods: {
    closeForm() {
      this.$emit("closeForm", false);
    },
    currentBoss(e){
        this.idOfBoss=e.target.value
       
    },
    addNewUser(e){
        e.preventDefault()
        if(!this.name){
            this.nameError=true
        }
        if(!this.number){
            this.numberError=true
        }
        if(!this.number || !this.name){
            return
        }
        this.$emit('addNewUser',{
            name:this.name,
            number:this.number,
            idOfBoss:this.idOfBoss
        })
    }
    ,
    AllBosses(users) {
      const a = (users) => {
        const result = [];
        users.forEach((el) => {
          result.push({ name: el.name, id: el.id });
          if(el.slaves.length){
            result.push(...a(el.slaves))
          }
        });
      
        return result
      
      };
      return a(users)
    },
  },
  watch: {
  users: {
    handler(newValue) {
      this.bosses=this.AllBosses(newValue)
    },
    immediate: true
  }
}
};
</script>

<template>
  <div class="form-wrapper">
    <span class="close" @click="closeForm">❌</span>
    <form  action="" class="formToAddContact">
      <label for="" >Имя</label>
      <input type="text" v-model="name" />
      <span v-if="nameError" class="error">Введите имя</span>
      <label for="">Номер</label>
      <input type="text" v-model="number" />
      <span v-if="numberError" class="error">Введите номер</span>
      <label for="">Начaльник</label>
      <select name="" id="" @change="currentBoss">
        <option value="" disabled selected>Выберите</option>
        <option v-for="el in bosses" :value="el.id">{{ el.name }}</option>
      </select>
      <button type="submit" @click="addNewUser">Сохранить</button>
    </form>
  </div>
</template>

<style scoped>
.formToAddContact {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 400px;
  background-color: antiquewhite;
  padding: 20px;
  gap: 10px;
}
.form-wrapper {
  position: relative;
}
.close {
  position: absolute;
  z-index: 1;
  margin-left: 400px;
  margin-top: 10px;
}
.error{
    color: red;
}
</style>
