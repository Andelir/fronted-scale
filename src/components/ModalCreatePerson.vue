<script>
import {defineComponent} from 'vue';

export default defineComponent({
  name: "ModalCreatePerson",
  emits: ["crear"],
  data() {
    return {
      personCreate: {
        firstName: '',
        secondName: '',
        firstLastName: '',
        secondLastName: '',
        email: ''
      }
    }
  },
  methods: {
    async createPerson() {
      const res = await fetch(
          `http://localhost:3000/api/v1/people`,
          {
            method: 'POST',
            headers: {
              "Content-Type" : "application/json"
            },
            body: JSON.stringify(this.personCreate),
            redirect: 'follow'
          }
      );
      const { person, message } = await res.json();
      //Envio la persona actualizada al padre(TablePeople) para refrescar la data.
      this.$emit('crear',person, message, 'create');
      this.personCreate = {};
      //Cierro el modal
      this.$refs.closeModal.click();
    },
  }
})
</script>

<template>
  <!-- Modal -->
  <div class="modal fade" id="createPerson" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
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
                  <input v-model="personCreate.firstName" type="text" class="form-control" id="firstName" >
                </div>
                <div class="mb-3 col-md-6">
                  <label for="secondName" class="form-label">Segundo Nombre</label>
                  <input v-model="personCreate.secondName" type="text" class="form-control" id="secondName"
                         placeholder="">
                </div>
              </div>
              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="firstLastName" class="form-label">Primer Apellido</label>
                  <input v-model="personCreate.firstLastName" type="text" class="form-control" id="firstLastName" placeholder="">
                </div>
                <div class="mb-3 col-md-6">
                  <label for="secondLastName" class="form-label">Segundo Apellido</label>
                  <input v-model="personCreate.secondLastName" type="text" class="form-control" id="secondLastName"
                         placeholder="">
                </div>
              </div>
              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="email" class="form-label">Correo Electrónico</label>
                  <input v-model="personCreate.email" type="email" class="form-control" id="email" placeholder="example@gmail.com">
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button ref="closeModal" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          <button type="button" class="btn btn-primary" @click="createPerson()">Crear</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>