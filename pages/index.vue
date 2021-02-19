<template>
  <div class="container my-5">
    <div class="row">
      <div
        v-for="(v, index) in $v.lists.$each.$iter"
        :key="index"
        class="col-md-3 rounded mt-4"
      >
        <div class="shadow p-3">
          <!-- Form lista - -->
          <form action="">
            <b-form-input
              id="inline-form-input-name"
              v-model.trim="v.name.$model"
              :class="v.$error ? 'is-invalid' : ''"
              class="mb-2 mr-sm-2 mb-sm-0"
              placeholder="Nombre de la lista"
            ></b-form-input>
            <div>
              <span v-if="!v.name.required && v.$error" class="invalid-text">
                El nombre de lista es requerido.
              </span>
              <span v-if="!v.name.maxLength && v.$error" class="invalid-text">
                El nombre de usuario no debe tener más de
                {{ v.name.$params.maxLength.max }} letras.
              </span>
            </div>
            <div class="d-flex">
              <b-button pill class="mt-2" variant="outline-success">
                Agregar lista
              </b-button>
              <div class="ml-3 mt-3 pointer">
                <i
                  class="pointer fas fa-times"
                  @click="lists.splice(index, 1)"
                ></i>
              </div>
            </div>
          </form>
          <!-- Fin Form lista -->
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
    }
  },
  validations: {
    lists: {
      $each: {
        name: {
          required,
          maxLength: maxLength(15),
        },
      },
    },
    prueba: {
      required,
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
  },
}
</script>
