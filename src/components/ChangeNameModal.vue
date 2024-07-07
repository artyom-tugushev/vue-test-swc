<template>
  <p-dialog
    v-model:visible="isModalShow"
    modal
    header="Edit Brand"
    :style="{ width: '25rem' }"
  >
    <div class="flex gap-2">
      <p-inputtext
        id="name"
        v-model="nameInput"
        class="flex-auto"
        autocomplete="off"
      />
      <p-button type="button" label="Save" @click="changeName"></p-button>
    </div>
  </p-dialog>
</template>

<script>
import Dialog from "primevue/dialog";
import Button from "primevue/button";
import InputText from "primevue/inputtext";

export default {
  emits: ["updateName"],
  props: {
    car: Object,
  },
  components: {
    "p-button": Button,
    "p-dialog": Dialog,
    "p-inputtext": InputText,
  },
  data() {
    return {
      isModalShow: false,
      nameInput: null,
      carId: null,
    };
  },
  methods: {
    openModal() {
      this.isModalShow = true;
      this.nameInput = this.$props.car.carName;
      this.carId = this.$props.car.carId;
    },
    changeName() {
      this.$emit("updateName", { name: this.nameInput, id: this.carId });
      this.isModalShow = false;
    },
  },
};
</script>

<style></style>
