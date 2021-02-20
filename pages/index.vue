<template>
  <div class="container my-5">
    <div class="row">
      <div
        v-for="(v, index) in $v.lists.$each.$iter"
        :key="index"
        class="col-md-3 rounded mt-4"
      >
        <div
          class="shadow -bg-grey"
          :class="lists[index].status === 'creating' ? 'p-3' : 'pb-2 pl-2'"
        >
          <!-- Lista 'saved' -->
          <div v-if="lists[index].status === 'saved'">
            <!-- Lista header -->
            <div>
              <!-- Titulo de la lista y botón de eliminar -->
              <div class="d-flex">
                <b-form-input
                  v-model.trim="v.name.$model"
                  class="-pointer mb-2 mr-sm-2 mb-sm-0"
                  style="
                    background-color: #e9ecef00 !important;
                    border: 1px solid #ced4da00 !important;
                  "
                  :readonly="titleList"
                  @click="!titleList"
                  :class="v.$error ? 'is-invalid' : ''"
                ></b-form-input>
                <div class="mt-2 col-2">
                  <i
                    class="-pointer fas fa-times"
                    @click="removeList(index)"
                  ></i>
                </div>
              </div>
              <!-- Fin titulo de la lista y botón de eliminar -->
              <!-- Validaciones del titulo de la lista -->
              <div>
                <span v-if="!v.name.required && v.$error" class="-invalid-text">
                  El nombre de lista es requerido.
                </span>
                <span
                  v-if="!v.name.maxLength && v.$error"
                  class="-invalid-text"
                >
                  El nombre de la lista no debe tener más de
                  {{ v.name.$params.maxLength.max }} letras.
                </span>
              </div>
              <!-- Fin validaciones del tiulo de la lista -->
              <!-- Botón de agregar una tarea -->
              <div class="mr-2">
                <b-button
                  class="fs-6"
                  variant="light"
                  size="lg"
                  @click="addCard(v, index)"
                  block
                >
                  <i class="fas fa-plus"></i> Agrega una tarea
                </b-button>
              </div>
              <!-- Fin botón de agregar un tarea -->
            </div>
            <!-- Fin lista header -->
            <!-- Lista body -->
            <div
              v-for="(vCard, indexCard) in $v.lists.cards.$each.$iter"
              :key="indexCard"
            >
              xd
            </div>
            <!-- Fin lista body -->
          </div>
          <!-- Fin lista 'saved' -->
          <!-- Form lista 'creating' - -->
          <form
            v-if="lists[index].status === 'creating'"
            @submit.prevent="saveList(v, index)"
          >
            <b-form-input
              v-model.trim="v.name.$model"
              :class="v.$error && v.cards.length === 0 ? 'is-invalid' : ''"
              class="mb-2 mr-sm-2 mb-sm-0"
              placeholder="Nombre de la lista"
            ></b-form-input>
            <div>
              <span v-if="!v.name.required && v.$error" class="-invalid-text">
                El nombre de lista es requerido.
              </span>
              <span v-if="!v.name.maxLength && v.$error" class="-invalid-text">
                El nombre de la lista no debe tener más de
                {{ v.name.$params.maxLength.max }} letras.
              </span>
            </div>
            <div class="d-flex">
              <b-button
                type="submit"
                pill
                class="mt-2"
                variant="outline-success"
              >
                Agregar lista
              </b-button>
              <div class="ml-3 mt-3">
                <i class="-pointer fas fa-times" @click="removeList(index)"></i>
              </div>
            </div>
          </form>
          <!-- Fin Form lista 'creating' -->
        </div>
      </div>
      <!-- Boton - Agregar lista -->
      <div class="col-md-3 mt-4">
        <b-button class="fs-6" variant="light" size="lg" @click="addList">
          Agregar lista <i class="fas fa-plus"></i>
        </b-button>
      </div>
      <!-- Fin boton agregar lista -->
    </div>
  </div>
</template>
<script>
import { required, maxLength } from 'vuelidate/lib/validators'
export default {
  data() {
    return {
      lists: [],
      titleList: false,
    }
  },
  validations: {
    lists: {
      $each: {
        cards: {
          title: {
            required,
          },
        },
        name: {
          required,
          maxLength: maxLength(15),
        },
      },
    },
  },
  methods: {
    addList() {
      const newList = {
        name: '',
        status: 'creating',
        cards: [],
      }
      this.lists.push(newList)
    },
    saveList(v, index) {
      v.$touch()
      if (!v.$invalid) {
        this.lists[index].status = 'saved'
      }
    },
    removeList(index) {
      this.lists.splice(index, 1)
    },
    addCard(list, index) {
      list.$touch()
      if (!list.$invalid) {
        const newCard = {
          title: '',
        }
        this.lists[index].cards.push(newCard)
      }
    },
  },
}
</script>
