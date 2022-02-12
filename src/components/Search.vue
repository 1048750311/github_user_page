<template>
    <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <input type="text" placeholder="enter the name you search" v-model="keyWord" @keyup.enter="SearchUsers"/>&nbsp;
        <button @click="SearchUsers" >Search</button>
      </div>
    </section>

</template>
<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyWord: ""
    };
  },
  methods: {
    SearchUsers() {
      this.$bus.$emit('updateListData',{isFirstShow:false,isLoading:true,errMsg:'',users:[]})
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        response => {
          console.log("请求成功", response.data.items);
          //请求成功后更新list的数据
          this.$bus.$emit('updateListData',{isFirstShow:false,isLoading:false,errMsg:'',users:response.data.items})
        },
        error => {
          console.log("失败", error.message);
          //请求失败后清空list的数据          
          this.$bus.$emit('updateListData',{isFirstShow:false,isLoading:false,errMsg: error.message,users:[]})
        }
      );
    }
  }
};
</script>
<style lang="css" scoped>
</style>
