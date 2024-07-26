<script>
import { onBeforeMount, onMounted } from "vue";

export default {
  name: "User",
  props: ["user", "count", "borderTop"],
  
  data() {
    return {
      somethn: "",
    };
  },
  methods: {
  
  },
  computed: {
    setPaddingLeft() {
      return {
        marginLeft: this.count * 10 + "px",
        borderTop: this.borderTop || "1px solid black",
        // borderBottom:
        //   this.user.slaves.length == 0 && this.count > 1
        //     ? "none"
        //     : "1px solid black",
      };
    },
    divider(){
        return {
            marginLeft: this.count>1?-this.count * 5+5 + "px":0
        }
    }
  },
};
</script>

<template>
  <div class="user-wrapper">
    <div class="user" :style="setPaddingLeft">
      <div class="name">{{ user.name }}</div>
      <div class="number">{{ user.number }}</div>
      <div class="divider" :style="divider"></div>
    </div>
    <User
      v-if="user.slaves.length"
      v-for="el in user.slaves"
      :user="el"
      :count="count + 1"
      
    />
  </div>
  <!-- :borderTop="'none'" -->
</template>

<style scoped>
.user {
  display: flex;
  flex-direction: row;
  
  border: 1px solid black;
  /* padding: 10px; */
  justify-content: space-between;
  position: relative;
  padding: 10px;

}

.name {
  /* border: 1px solid black;
    height: 100%;
    width: 100%; */
    width: 100px;

  flex: 1;
 
}
.number {
  /* border-left: 1px solid black; */
    display: flex;
    justify-content: flex-end;
    margin-right: 10px;
  flex: 1;

}

.user-wrapper {
  display: flex;
  flex-direction: column;
}

</style>
