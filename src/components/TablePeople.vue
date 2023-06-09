<script>
import { defineComponent } from "vue";
import ModalEditPerson from "@/components/ModalEditPerson.vue";
import ModalDeletePerson from "@/components/ModalDeletePerson.vue";
import ModalCreatePerson from "@/components/ModalCreatePerson.vue";
import { toast } from "vue3-toastify";

export default defineComponent({
  name: "TablePeople.vue",
  components: { ModalDeletePerson, ModalEditPerson, ModalCreatePerson },
  /*   setup() {
    const notify = () => {
      toast.success("Persona actualizada correctamente !", {
        autoClose: 1000,
        position: toast.POSITION.BOTTOM_RIGHT,
      }); // ToastOptions
    }
    return { notify };
   }, */
  data() {
    return {
      people: [
        {
          id: "",
          firstName: "",
          secondName: "",
          firstLastName: "",
          secondLastName: "",
          email: "",
        },
      ],
      personEdit: {},
      actions: {
        create : (person) => this.people.push(person),
        update : (person) => this.people = this.people.map((p) => (p.id === person.id ? person : p)),
        delete : (person) => this.people = this.people.filter((p) => p.id !== person.id),
      }
    };
  },
  methods: {
    async getPeople() {
      this.people = null;
      const res = await fetch(`http://localhost:3000/api/v1/people`, {
        method: "GET",
      });
      this.people = await res.json();
    },
    updatePersonEdit(person) {
      this.personEdit = person;
    },
    refreshData(person, message, action) {
      //* Notificación
      console.log('Persona: ', JSON.parse(JSON.stringify(person)));
      console.log('Mensaje: ', message);
      console.log('Acción: ', action);
      this.notify(message);
      const personData = JSON.parse(JSON.stringify(person));
      this.actions[action](personData);
    },
    notify(message) {
      toast.success(message, {
        autoClose: 1000,
        position: toast.POSITION.BOTTOM_RIGHT,
      }); // ToastOptions
    },
  },
  mounted() {
    this.getPeople();
  },
});
</script>

<template>
  <div class="container" id="table-people">
    <!------Modals------------>
    <ModalEditPerson :personProp="personEdit" @actualizar="refreshData" />
    <ModalDeletePerson :personProp="personEdit" @eliminar="refreshData" />
    <ModalCreatePerson @crear="refreshData" />
    <!------------------------>
    <h3>Personas</h3>
    <button
      type="button"
      class="btn btn-outline-primary"
      data-bs-toggle="modal"
      data-bs-target="#createPerson"
    >
      Agregar Persona
    </button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col"><b>#</b></th>
          <th scope="col"><b>Nombres</b></th>
          <th scope="col"><b>Apellidos</b></th>
          <th scope="col"><b>Correo Electrónico</b></th>
          <th scope="col"><b>Acciones</b></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in people" :key="person.id">
          <th scope="row">{{ person.id }}</th>
          <td align="center">
            {{ person.firstName.concat(" ", person.secondName) }}
          </td>
          <td align="center">
            {{ person.firstLastName.concat(" ", person.secondLastName) }}
          </td>
          <td align="center">{{ person.email }}</td>
          <td>
            <div class="d-flex justify-content-center" style="gap: 10px">
              <button
                @click="updatePersonEdit(person)"
                type="button"
                class="btn btn-primary"
                data-bs-toggle="modal"
                data-bs-target="#editPerson"
              >
                Editar
              </button>
              <button
                @click="updatePersonEdit(person)"
                type="button"
                class="btn btn-danger"
                data-bs-toggle="modal"
                data-bs-target="#deletePerson"
              >
                Eliminar
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
th {
  text-align: center;
}
</style>
