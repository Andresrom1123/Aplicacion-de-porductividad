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
          :class="lists[index].status === 'creating' ? 'p-3' : ''"
        >
          <!-- Lista 'saved' -->
          <div v-if="lists[index].status === 'saved'">
            <!-- Lista header -->
            <div
              id="list-header"
              :class="v.$model.cards.length > 0 ? 'border-bottom' : ''"
            >
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
                  :class="v.name.$error ? 'is-invalid' : ''"
                  @click="!titleList"
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
            </div>
            <!-- Fin lista header -->
            <!-- Lista body -->
            <div
              id="list-body"
              :class="v.$model.cards.length > 0 ? 'px-3 pt-3 pb-0' : ''"
            >
              <div
                v-for="(vCard, indexCard) in v.cards.$each.$iter"
                :key="indexCard"
              >
                <div class="d-flex">
                  <b-form-textarea
                    id="textarea"
                    v-model="vCard.title.$model"
                    placeholder="Titulo de la tarjeta..."
                    rows="1"
                    max-rows="4"
                    class="col-10 overflow-hidden"
                    style="background-color: #ffffff !important"
                    :class="[
                      { 'is-invalid': vCard.title.$error },
                      {
                        '-pointer': lists[index].cards[indexCard].status,
                      },
                    ]"
                  ></b-form-textarea>
                  <i
                    class="col-1 -pointer fas fa-times text-muted"
                    @click="removeCard(index, indexCard)"
                  >
                  </i>
                </div>
                <div class="mb-3">
                  <span
                    v-if="!vCard.title.required && vCard.title.$error"
                    class="-invalid-text"
                  >
                    El titulo de la tarjeta es requerido.
                  </span>
                </div>
              </div>
            </div>
            <!-- Fin lista body -->
            <!-- Botón de agregar una tarea -->
            <div>
              <b-button
                class="fs-6"
                variant="light"
                size="lg"
                block
                @click="addCard(v, index)"
              >
                <i class="fas fa-plus"></i> Agrega una tarea
              </b-button>
            </div>
            <!-- Fin botón de agregar un tarea -->
          </div>
          <!-- Fin lista 'saved' -->
          <!-- Form lista 'creating' - -->
          <form
            v-if="lists[index].status === 'creating'"
            @submit.prevent="saveList(v, index)"
          >
            <b-form-input
              v-model.trim="v.name.$model"
              :class="v.name.$error ? 'is-invalid' : ''"
              class="mb-2 mr-sm-2 mb-sm-0"
              placeholder="Nombre de la lista"
            ></b-form-input>
            <div>
              <span
                v-if="!v.name.required && v.name.$error"
                class="-invalid-text"
              >
                El nombre de lista es requerido.
              </span>
              <span
                v-if="!v.name.maxLength && v.name.$error"
                class="-invalid-text"
              >
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
          $each: {
            title: {
              required,
            },
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
      if (!list.name.$invalid && !list.cards.$invalid) {
        const newCard = {
          title: '',
          status: false,
        }
        this.lists[index].cards.push(newCard)
        const cardsLength = this.lists[index].cards.length
        if (cardsLength > 1) {
          this.lists[index].cards[cardsLength - 2].status = true
        }
      }
    },
    removeCard(indexList, indexCard) {
      this.lists[indexList].cards.splice(indexCard, 1)
    },
  },
}
</script>
