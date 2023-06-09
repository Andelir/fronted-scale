<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "ModalDeletePerson",
  emits: ["eliminar"],
  props: {
    personProp: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      personDelete: { firstName: '' },
    };
  },
  watch: {
    personProp(newValue, oldValue) {
      this.personDelete = JSON.parse(JSON.stringify(newValue));
    },
  },
  methods: {
    async deletePerson() {
      const res = await fetch(
        `http://localhost:3000/api/v1/people/${this.personDelete.id}`,
        {
          method: "DELETE",
          redirect: "follow",
        }
      );
      const { message } = await res.json();
      //Envio la persona eliminada al padre(TablePeople) para refrescar la data.
      this.$emit("eliminar", this.personDelete, message, 'delete');
      //Cierro el modal
      this.$refs.closeModal.click();
    },
  },
});
</script>

<template>
  <!-- Modal -->
  <div
    class="modal fade"
    id="deletePerson"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">
            Eliminar Persona
          </h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <form>
            <div class="container">
              <div class="row">
                <div class="col">
                  <p>
                    ¿ Estas seguro de eliminar la persona
                    <b>{{
                      personDelete.firstName.concat(
                        ' ',
                        ' ' + personDelete.secondName,
                        ' ' + personDelete.firstLastName,
                        ' ' + personDelete.secondLastName
                      )
                    }}
                    ?
                </b>
                  </p>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button
            ref="closeModal"
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            No
          </button>
          <button type="button" class="btn btn-danger" @click="deletePerson()">
            Si
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
