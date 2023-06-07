<script>
import {defineComponent} from 'vue';
import EditPerson from "@/components/EditPerson.vue";
import DeletePerson from "@/components/DeletePerson.vue";

export default defineComponent({
  name: "TablePeople.vue",
  components: {DeletePerson, EditPerson},

  data() {
    return {
      people: [{
        'id':'',
        'firstName' : '',
        'secondName' : '',
        'firstLastName' : '',
        'secondLastName' : '',
        'email' : ''
      }]
    }
  },
  methods: {
    async getPeople() {
      this.people = null
      const res = await fetch(
          `http://localhost:3000/api/v1/people`,
          {
            method: 'GET'
          }
      );
      this.people = await res.json();
    },
    refreshData(x) {
      console.log( JSON.parse(JSON.stringify(x)), 'entre');
      /*this.people = this.people.map( (p) => {
        p.id === person.id ? person : p
      })*/
    }
  },
  mounted() {
    this.getPeople();
  }
})
</script>

<template>
  <div class="container" id="table-people">



    <h3>Personas</h3>
    <button type="button" class="btn btn-outline-primary">Agregar Persona</button>
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
          <td align="center">{{ person.firstName.concat(' ', person.secondName) }}</td>
          <td align="center">{{ person.firstLastName.concat(' ', person.secondLastName) }}</td>
          <td align="center">{{ person.email }}</td>
          <td>
            <div class="d-flex justify-content-center" style="gap: 10px">
              <EditPerson @refresh-data="refreshData" :person="person"/>
              <DeletePerson/>
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