<template>
  <div class="wrapper">
    <div class="search-block">
      Поиск <input type="text" class="search" v-model="search"  autofocus />
    </div>
    <table class="table-student">
      <thead>
        <tr>
          <th v-for="column in columns">
            {{ column  }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(list, index) in currentLists" :key="index">
          <th><input type="checkbox" class="list-item-checkbox" v-model="list.isChecked" /></th>
          <th >
            {{list.name}}
          </th>
          <th>
              {{list.family}}
          </th>
          <th>{{list.data}}</th>
          <th>{{list.group}}</th>
          <th>
            <button  class="btn-delet"  @click="showeditModal(index)">Изменить</button>
          </th>
        </tr>
      </tbody>
      <button @click="deleteItems()" class="btn-delet">Удалить</button>
    </table>
  </div>
</template>



<script>


export default {
  props: {
    initLists: {
      required: true,
      type: Array 
    },

  },
  created(){
    this.$root.$on('saveItem', (item, index) => {
      console.log(index);
      console.log(item);
      if(index == null){
        this.lists.push(item)
      }else{
        this.currentLists[index] = item
      }
    }) 
    this.lists = this.initLists
    document.addEventListener('keydown', ({code}) => { 
    if (code !== 'Delete' && code !== 'Backspace') {
      return
    }
      this.deleteItems()
    });
  },
  data(){
    return {
      columns: [ '', 'Имя', 'Фамилия', 'Возраст', 'Группа'],
      search: '',
      lists: null
    }
  },
  methods: {
    // addItem: function () {
    //   this.lists.push({
    //     name: this.modalName,
    //     family: this.modalFamily, 
    //     data: this.modalData,
    //     group: this.modalGroup,
    //     isChecked: false,
    //     edit: false
    //   })
    //   this.modalName = '';
    //   this.modalFamily = '';
    //   this.modalData = '';
    //   this.modalGroup = '';
    //},
    deleteItems: function() {
    this.currentLists = this.currentLists.filter(function(list) {
        return list.isChecked === false
      })
    },
    showeditModal: function (index){
      this.$emit('edit_student', this.currentLists[index], index);
    },
    editItem: function(list){
      this.isOpen = true;
      // this._originalItem = Object.assign({}, list);
      // list.edit = true;
    },
    cancelItem: function(list){
      Object.assign(list, this._originalItem);
      list = this._originalItem
      //this.item = _originalItem;
      list.edit = false;
    },
  },
  computed: {
    filteredItems: function(){
      return this.lists.filter(list => list.family.toLowerCase().includes(this.search.toLowerCase()));
    },
    currentLists (){
      return this.filteredItems
      //return (this.search == '' ? this.lists : this.filteredItems)
      //return this.lists
    }
  }
  

}
</script>

<style scoped>
  .table-student{
    max-width: 700px;
    width: 100%;
    margin: 0 auto;
    padding-top: 50px;
  }
  .search-block{
    margin-top: 30px;
  }
</style>