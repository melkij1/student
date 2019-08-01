<template>
  <div id="app">
    <button @click="isOpen=!isOpen">Добавить студента</button>
    <ModalForm v-if="isOpen" @saveItem="saveItem" @close="closeModel" :model-index="modelIndex" :init-model="model"/>
    <TableStudent  modal="false" :init-lists="lists"  @edit_student="edit_student_method" />
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import ModalForm from './components/ModalForm.vue'
import TableStudent from './components/TableStudent.vue'


export default {
  name: 'app',
  data(){
    return {
      lists: [
        { name: "Вася", family: "Петров", data: "06.10.1993", group: "ИВТ-1-15", isChecked: false, edit: false },
        { name: "Иван", family: "Пупкин", data: "10.06.1990", group: "ИВТ-3-135", isChecked: false, edit: false },
      ],
      isOpen: false,
      model: {},
      modelIndex: null
    }
  },
  components: {
    HelloWorld,
    ModalForm,
    TableStudent
  },
  methods: {
    saveItem(item, index){
      this.$root.$emit('saveItem', item, index)
      this.model = {}
      this.modelIndex = null
    },
    edit_student_method(model, index) {
      console.log(model);
      console.log(index);
      this.isOpen = true;
      this.modelIndex= index;
      this.model = model
    },
    closeModel(){
      this.isOpen = false;
      this.model = {}
      this.modelIndex = null
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
