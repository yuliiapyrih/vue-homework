<template>
  <div>
  <input type="text" v-model='list'>
  <button @click='save' v-show='show'>Save point</button>
  
  <ul>
      <li v-for="(point, indexList) in todolist" :key='indexList'>
        {{point}} 
         <button @click='remove(indexList)'>Delete point</button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      todolist:[],
      list:'',
      show: true,
      id_index:0
    }
  },

  methods:{
    
    save(){
      this.todolist.push(this.list);
      this.$http.post('https://myhw-vue.firebaseio.com/todolist.json',{list:this.list});
      this.list='';
      this.todolist.length>9 ? this.show=false : this.show=true;
    },
    remove(indexList){
      this.todolist.splice(indexList,1);
      this.$http.get('https://myhw-vue.firebaseio.com/todolist.json')
      .then((res)=>{
        return res.json();
      }).then((res)=>{

        console.log(Object.keys(res)[indexList]);
        this.$http.delete(`https://myhw-vue.firebaseio.com/todolist/${Object.keys(res)[indexList]}.json`)
      });
      this.todolist.length>9 ? this.show=false : this.show=true;
    }
  },
  beforeCreated(){
    console.log('before');
  }

  
}
</script>

<style>
</style>
