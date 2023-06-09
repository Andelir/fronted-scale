<script>

import {defineComponent} from 'vue';

export default defineComponent({
  name: "ModalEditPerson",
  emits: ["actualizar"],
  props: {
    personProp: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      personEdit: {...this.personProp}
    }
  },
  beforeUpdate() {
    // Acceder al estado anterior y al estado actual
    //this.personEditOf = {...this.personEditOp}
    //console.log('Estado anterior:', this.personEditOf);
    //console.log('Estado actual:', this.personEditOf);
    //console.log('Estado anterior:', this.$data);
    //console.log('Estado actual:', this.$data);
  },
  updated() {
    // Realizar acciones después de los cambios de estado
    //this.personEditOf = {...this.personEditOp}
    //console.log(t'El componente ha cambiado su estado.');
  },
  watch: {
     personProp(newValue, oldValue) {
/*       console.log('Nuevo valor de personEditOp', newValue);
      console.log('Antiguo valor de personEditOp', oldValue); */
      this.personEdit = JSON.parse(JSON.stringify(newValue));
    },
  /*   personEditOf(nuevoValor, valorAnterior) {
      console.log('Propiedad cambiada. Nuevo valor of:', nuevoValor);
      console.log('Propiedad cambiada. Valor anterior of:', valorAnterior);
    }, */
  },
  methods: {
    async updatePerson() {
      const res = await fetch(
          `http://localhost:3000/api/v1/people/${this.personEdit.id}`,
          {
            method: 'PUT',
            headers: {
              "Content-Type" : "application/json"
            },
            body: JSON.stringify(this.personEdit),
            redirect: 'follow'
          }
      )
      const { message } = await res.json();
      //Envio la persona actualizada al padre(TablePeople) para refrescar la data.
      this.$emit('actualizar',this.personEdit, message, 'update');
      //Cierro el modal
      this.$refs.closeModal.click();
      
    },
  }
})
</script>

<template>
  <!-- Modal -->
  <div class="modal fade" id="editPerson" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Editar Persona</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <form>
            <div class="container">
              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="firstName" class="form-label">Primer Nombre</label>
                  <input v-model="personEdit.firstName" type="text" class="form-control" id="firstName" >
                </div>
                <div class="mb-3 col-md-6">
                  <label for="secondName" class="form-label">Segundo Nombre</label>
                  <input v-model="personEdit.secondName" type="text" class="form-control" id="secondName"
                         placeholder="">
                </div>
              </div>
              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="firstLastName" class="form-label">Primer Apellido</label>
                  <input v-model="personEdit.firstLastName" type="text" class="form-control" id="firstLastName" placeholder="">
                </div>
                <div class="mb-3 col-md-6">
                  <label for="secondLastName" class="form-label">Segundo Apellido</label>
                  <input v-model="personEdit.secondLastName" type="text" class="form-control" id="secondLastName"
                         placeholder="">
                </div>
              </div>
              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="email" class="form-label">Correo Electrónico</label>
                  <input v-model="personEdit.email" type="text" class="form-control" id="email" placeholder="example@gmail.com">
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button ref="closeModal" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          <button type="button" class="btn btn-primary" @click="updatePerson()">Guardar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>