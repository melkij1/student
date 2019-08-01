<template>
   <div>
     <transition name="modal">
      <div>
          <div class="overlay" @click.self="closeModal">
            <div class="modal">
              <div class="modal-form">
                <div class="form-control">
                  <input type="text" v-model="model.name" name="name" placeholder="Имя">
                </div>
                <div class="form-control">
                  <input type="text" v-model="model.family" name="family" placeholder="Фамилия">
                </div>
                <div class="form-control">
                  <input type="text" v-model="model.data" placeholder="Возраст">
                </div>
                <div class="form-control">
                  <input type="text" v-model="model.group" placeholder="Группа">
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" @click="saveItem" @click.self="isOpen = false;">Добавить</button>

              </div>
            </div>

          </div>
      </div>
      </transition>
   </div>
</template>


<script>

export default{
  props: {
    initModel: {
      required: false,
      type: Object,
      default(){
        return {}
      } 
    },
    modelIndex: {
      required: false,
      type: Number
    },
  },
  created() {
    console.log(this.modeldata);
    if(this.initModel == {}){
      return 
    }
    this.model = this.initModel
  },
  data() {
    return {
      isOpen: false,
    };
  },


  methods:{
    saveItem(){
      this.$emit('saveItem', this.model, this.modelIndex);
      this.model = this.defaultModel()
      this.closeModal()
    },
    closeModal(){
      this.$emit('close')
    },
    defaultModel(){
      return {
        name: '',
        family: '',
        data: '',
        group: '',
        isChecked: false,
        edit: false
      }
    },
  }
}


</script>


<style scoped>
.modal {
  width: 500px;
  margin: 0px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px 3px;
  transition: all 0.2s ease-in;
  font-family: Helvetica, Arial, sans-serif;
}
.fadeIn-enter {
  opacity: 0;
}

.fadeIn-leave-active {
  opacity: 0;
  transition: all 0.2s step-end;
}

.fadeIn-enter .modal,
.fadeIn-leave-active.modal {
  transform: scale(1.1);
}
button {
  padding: 7px;
  margin-top: 10px;
  background-color: green;
  color: white;
  font-size: 1.1rem;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #00000094;
  z-index: 999;
  transition: opacity 0.2s ease;
}
.form-control{
  margin-bottom: 15px;
}
input{
  height: 20px;
}
</style>