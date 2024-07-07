<template>
  <div class="container">
    <div class="card flex justify-content-center mt-5">
      <p-dropdown
        v-model="selectedFilter"
        :options="filters"
        placeholder="Sort"
        class="w-full border-2 shadow-1 md:w-14rem"
      />
    </div>

    <div class="grid mt-3">
      <div
        class="col-12 sm:col-6 lg:col-4"
        v-for="car of sortedData"
        :key="car.id"
      >
        <div
          class="flex flex-column p-5 surface-0 border-2 border-300 border-round-lg gap-2"
        >
          <div class="flex gap-2 align-items-center">
            <div class="flex flex-wrap gap-1">
              <div
                @click="openNameModal(car.name, car.id)"
                class="text-xl text-secondary font-semibold cursor-pointer"
              >
                {{ car.name }}
              </div>
              <div
                @click="openModelModal(car.model, car.id)"
                class="text-xl text-primary font-semibold cursor-pointer"
              >
                {{ car.model }}
              </div>
            </div>
            <div class="text-lg ml-auto">{{ car.year }}</div>
          </div>

          <div
            @click="openPriceModal(car.price, car.id)"
            class="text-xl text-secondary font-semibold cursor-pointer"
          >
            {{ `$ ${car.price.toLocaleString("ru-RU")}` }}
          </div>

          <div class="flex gap-2 align-items-center">
            <div
              class="border-round color-icon"
              :class="[
                car.color === 'black' ? 'surface-900' : '',
                car.color === 'silver' ? 'surface-300' : `bg-${car.color}-500`,
              ]"
            ></div>
            <div>
              {{ car.color }}
            </div>
          </div>

          <div class="mt-2">
            <GoogleMap
              api-key="AIzaSyB7bEXreK2RKXtXbK0SWchgy7CLqCbzGhY"
              style="width: 100%; height: 300px"
              :zoom="12"
              :center="{ lat: car.latitude, lng: car.longitude }"
              ><Marker
                :options="{
                  position: { lat: car.latitude, lng: car.longitude },
                }"
              />
            </GoogleMap>
          </div>
        </div>
      </div>
    </div>

    <div class="grid my-6">
      <div
        class="col-12 sm:col-6 sm:col-offset-3 lg:col-4 lg:col-offset-4 py-0"
      >
        <div class="text-300 text-center mb-3">
          Test assignment from
          <a
            class="text-300 text-center mb-3"
            target="_blank"
            href="https://t.me/artyomtugushev"
            >Artyom Tugushev</a
          >
        </div>
      </div>
    </div>
  </div>

  <change-name-modal
    ref="changeNameModal"
    :car="selectedCar"
    @updateName="onUpdateName"
  >
    ></change-name-modal
  >

  <change-name-modal
    ref="changeModelModal"
    :car="selectedCar"
    @updateName="onUpdateModel"
  >
    ></change-name-modal
  >

  <change-name-modal
    ref="changePriceModal"
    :car="selectedCar"
    @updateName="onUpdatePrice"
  >
    ></change-name-modal
  >
</template>

<script>
import axios from "axios";
import Dropdown from "primevue/dropdown";
import Dialog from "primevue/dialog";
import InputText from "primevue/inputtext";
import InputNumber from "primevue/inputnumber";
import Button from "primevue/button";
import { GoogleMap, Marker } from "vue3-google-map";

import ChangeNameModal from "./components/ChangeNameModal.vue";

export default {
  components: {
    "p-dropdown": Dropdown,
    "p-dialog": Dialog,
    "p-button": Button,
    "p-inputtext": InputText,
    "p-inputnumber": InputNumber,
    GoogleMap,
    Marker,
    ChangeNameModal,
  },
  data() {
    return {
      search: "",
      data: [],
      filters: ["price ↑", "price ↓", "year", "no filter"],
      selectedFilter: null,

      selectedCar: { carName: null, carId: null },
    };
  },
  computed: {
    sortedData() {
      if (this.selectedFilter === "year") {
        return this.data.sort((a, b) => b.year - a.year);
      } else if (this.selectedFilter === "price ↑") {
        return this.data.sort((a, b) => a.price - b.price);
      } else if (this.selectedFilter === "price ↓") {
        return this.data.sort((a, b) => b.price - a.price);
      } else if (!this.selectedFilter || this.selectedFilter === "no filter") {
        return this.data.sort((a, b) => b.id - a.id);
      }
    },
  },
  mounted() {
    this.getData();
  },
  methods: {
    openNameModal(name, id) {
      this.selectedCar.carName = name;
      this.selectedCar.carId = id;
      this.$refs.changeNameModal.openModal();
    },
    openModelModal(name, id) {
      this.selectedCar.carName = name;
      this.selectedCar.carId = id;
      this.$refs.changeModelModal.openModal();
    },
    openPriceModal(name, id) {
      this.selectedCar.carName = name;
      this.selectedCar.carId = id;
      this.$refs.changePriceModal.openModal();
    },

    onUpdateName(data) {
      const car = this.data.find((item) => item.id === data.id);
      car.name = data.name;
    },
    onUpdateModel(data) {
      const car = this.data.find((item) => item.id === data.id);
      car.model = data.name;
    },
    onUpdatePrice(data) {
      const car = this.data.find((item) => item.id === data.id);
      car.price = data.name;
    },

    deleteCar(id) {
      this.data = this.data.filter((item) => item.id !== id);
    },

    async getData() {
      try {
        const response = await axios.get(
          "https://test.tspb.su/test-task/vehicles"
        );
        this.data = response.data;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.color-icon {
  border: 2px solid #dfdfdf;
  height: 24px;
  width: 24px;
}
</style>
