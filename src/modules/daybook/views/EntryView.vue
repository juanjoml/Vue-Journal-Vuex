<template>
  <div class="entry-title d-flex justify-content-between p-2">
    <div>
      <span class="text-success fs-3 fw-bold">{{day}}</span>
      <span class="mx-1 fs-3">{{month}}</span>
      <span class="mx-2 fs-4 fw-light">{{yearDate}}</span>
    </div>
    <div>
      <input type="file" @change="onSelectedImage">
      <button class="btn btn-danger mx-2" v-if="entry.id" @click="onDeleteEntry">
        Borrar
        <i class="fa fa-trash-alt"></i>
      </button>
      <button class="btn btn-primary">
        Subir foto
        <i class="fa fa-upload"></i>
      </button>
    </div> 
  </div>
  <hr>
  <div class="d-flex flex-column px-3 h-75">
    <textarea placeholder="¿Qué sucedió hoy?" v-model="entry.text"></textarea>
  </div>
  <Fab icon="fa-save" @on:click="saveEntry"/>
  <img src="https://www.65ymas.com/uploads/s1/58/60/1/bigstock-playa-las-catedrales-catedrais-262817350-1.jpeg" alt="entry-picture" class="img-thumbnail">
</template>

<script>
import { defineAsyncComponent } from "vue";
import { mapGetters, mapActions } from 'vuex'
import getDayMonthYear from "@/modules/daybook/helpers/getDayMonthYear";
const Swal = require('sweetalert2')
export default {
  props: {
        id: {
            type: String,
            required: true
        }
    },
  components: {
    Fab: defineAsyncComponent(() => import('@/modules/daybook/components/Fab.vue'))
  },
  data(){
    return {
      entry: null
    }
  },
  computed:{
    ...mapGetters('journal', ['getEntryById']),
    day(){
      const {day} = getDayMonthYear(this.entry.date)
      return day
    },
    month(){
      const {month} = getDayMonthYear(this.entry.date)
      return month
    },
    yearDate(){
      const {yearDate} = getDayMonthYear(this.entry.date)
      return yearDate
    }
  },
  methods: {
    ...mapActions('journal', ['updateEntry', 'createEntry', 'deleteEntry']),
    loadEntry(){
      let entry
      if(this.id === 'new'){
        entry = {
          text: '',
          date: new Date().getTime()
        }  
      }else{
      entry = this.getEntryById(this.id)
      if(!entry) return this.$router.push({name: 'no-entry'})
      }
      this.entry = entry
    },
    async saveEntry(){
      new Swal({
        title: 'Espere por favor',
        allowOutsideClick: false
      })
      Swal.showLoading()
      if(this.entry.id){
        await this.updateEntry(this.entry)
      }else{
        const id = await this.createEntry(this.entry)
        this.$router.push({name: 'entry', params: {id: id}})
      }
      Swal.fire('Guardado', 'Entrada registrada con éxito', 'success')
    },
    async onDeleteEntry(){
        const {isConfirmed} = await Swal.fire({
          title: '¿Está seguro?',
          text: 'Una vez borrado, no se puede recuperar',
          showDenyButton: true,
          confirmButtonText: 'Sí, estoy seguro'
        })
        if(isConfirmed){
          new Swal({
            title: 'Espere por favor',
            allowOutsideClick: false
          })
          Swal.showLoading()
          await this.deleteEntry(this.entry.id)
          this.$router.push({name: 'no-entry'})
          Swal.fire('Eliminado', '', 'success')
        }
      }
  },
  onSelectedImage(event){
    console.log(event)
  },
  created() {
    // console.log(this.$route.params.id)
    this.loadEntry()
  },
  watch: {
    id(){
      this.loadEntry()
    }
  }
}
</script>

<style lang="scss" scoped>

textarea{
  font-size: 20px;
  border: none;
  height: 100%;

  &:focus{
    outline: none;
  }
}
  img{
    width: 200px;
    position: fixed;
    bottom: 150px;
    right: 20px;
    box-shadow: 0px 5px 10px rgba($color: #000000, $alpha: 0.2);
  }

</style>