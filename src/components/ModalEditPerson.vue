<script>
import {defineComponent} from 'vue'

export default defineComponent({
  name: "ModalEditPerson"
})
</script>

<template>
<div>
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
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          <button type="button" class="btn btn-primary" @click="editPerson()">Guardar</button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<style scoped>

</style>