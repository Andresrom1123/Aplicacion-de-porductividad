<template>
  <div class="container my-5">
    <b-button class="fs-6" variant="light" size="lg">
      Agregar lista <i class="fas fa-plus"></i>
    </b-button>
    <div class="shadow mt-2 p-3 rounded" style="width: 90%; max-width: 15rem">
      <b-form-input
        id="inline-form-input-name"
        class="mb-2 mr-sm-2 mb-sm-0"
        placeholder="Nombre de la lista"
      ></b-form-input>
      <div class="d-flex">
        <b-button pill class="mt-2" variant="outline-success">
          Agregar lista
        </b-button>
        <div class="ml-3 mt-3 pointer">
          <i class="pointer fas fa-times"></i>
        </div>
      </div>
    </div>
  </div>
</template>
