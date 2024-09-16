<template>
  <main-menu></main-menu>
  <!-- хэдер -->
  <div class="header align-items-center flex my-4">
    <div class="dm-sans-700 text-3xl">Vehichles</div>
    <p-button
      class="ml-3"
      :label="data.length.toString()"
      severity="secondary"
    />
    <div class="ml-auto flex align-items-center mr-4">
      <p-button
        class="header-button mr-4 red-border"
        icon="pi pi-plus"
        severity="danger"
        outlined
        aria-label="Cancel"
      />
      <div class="flex align-items-center">
        <img
          class="profile-pic mr-2"
          src="./assets/images/user_photo.jpg"
          alt=""
        />
        <div class="dm-sans-500">John Doe</div>
      </div>
    </div>

    <p-dropdown
      v-model="selectedLang"
      :options="languages"
      optionLabel="lang"
      placeholder="Select a City"
      class="max-w-6rem md:w-14rem"
    />
  </div>

  <div class="border-05 border-300 my-2"></div>

  <!-- серчбар и дропдаун для пагинатора -->
  <div class="container default-layout">
    <div class="grid mt-4">
      <div class="col-12 sm:col-6 lg:col-4">
        <p-inputtext
          type="number"
          class="w-full"
          placeholder="Search VIN"
          v-model="vinSearch"
        >
        </p-inputtext>
      </div>
      <div class="col-12 sm:col-6 lg:col-4">
        <div class="flex">
          <div class="dm-sans-500 mr-1">Select vehicles per page:</div>
          <p-dropdown
            v-model="rowsPerPage"
            :options="rowsOptions"
            class="max-w-6rem md:w-14rem"
          />
        </div>
      </div>
      <div class="col-12 sm:col-6 lg:col-4 flex">
        <p-button
          label="ADD VEHICLE"
          class="ml-auto red-button"
          icon="pi pi-plus"
          severity="danger"
          aria-label="Cancel"
        />
      </div>

      <!-- отображение карточек с машинами -->
      <div
        class="mt-4 col-12 sm:col-6 lg:col-4"
        v-for="car in paginatedCars"
        :key="car.id"
      >
        <div class="p-3 surface-100 border-round-lg">
          <div class="flex mb-1">
            <img
              class="ml-auto"
              src="./assets/images/more_horizontal.svg"
              alt=""
            />
          </div>
          <img class="car-preview" :src="car.placeholder" alt="" />

          <div class="dm-sans-700 mt-2">
            {{
              car.vehicle_name && car.vehicle_name !== "undefined undefined"
                ? car.vehicle_name.toUpperCase()
                : "Unknown Car".toUpperCase()
            }}
          </div>
          <div v-if="car.vin == +car.vin" class="dm-sans-500 text-500 mt-2">
            VIN
            {{ car.vin }}
          </div>
          <div class="border-05 border-300 my-3"></div>
          <div class="flex align-items-center dm-sans-500">
            <div
              class="inline-block p-1 border-round-sm surface-200"
              :class="{ checked: +car.angles_count === 16 }"
            >
              <div class="flex gap-1">
                <img
                  v-if="+car.angles_count === 16"
                  class="pl-1"
                  src="./assets/images/check.svg"
                  alt=""
                />
                <div class="px-1">{{ car.angles_count }}/16</div>
              </div>
            </div>
            <div class="ml-auto text-500">3 days left</div>
          </div>
        </div>
      </div>
    </div>
    <div class="flex align-items-center mt-3 mb-5">
      <div class="dm-sans-500">
        Showing {{ filteredData.length }} out of {{ data.length }}
      </div>

      <p-paginator
        class="ml-auto"
        :rows="rowsPerPage"
        :totalRecords="filteredData.length"
        @page="onPageChange"
      >
      </p-paginator>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Dropdown from "primevue/dropdown";
import Dialog from "primevue/dialog";
import InputText from "primevue/inputtext";
import InputNumber from "primevue/inputnumber";
import Button from "primevue/button";
import Paginator from "primevue/paginator";

import MainMenu from "./components/MainMenu.vue";

export default {
  components: {
    "p-dropdown": Dropdown,
    "p-dialog": Dialog,
    "p-button": Button,
    "p-inputtext": InputText,
    "p-inputnumber": InputNumber,
    "p-paginator": Paginator,
    MainMenu,
  },
  data() {
    return {
      data: [],
      filteredData: [],
      vinSearch: null,

      rowsPerPage: 9,
      currentPage: 1,
      rowsOptions: [9, 20],

      selectedLang: { lang: "En", code: "eng" },
      languages: [
        { lang: "En", code: "eng" },
        { lang: "Ru", code: "rus" },
      ],
    };
  },
  computed: {
    paginatedCars() {
      const start = (this.currentPage - 1) * this.rowsPerPage;
      const end = start + this.rowsPerPage;
      return this.filteredData.slice(start, end);
    },
  },
  watch: {
    vinSearch() {
      this.filterData();
    },
    rowsPerPage() {
      this.currentPage = 1;
    },
  },
  methods: {
    async getData() {
      try {
        const response = await axios.get(
          "https://api.caiman-app.de/api/cars-test"
        );
        this.data = response.data.data;
        this.filteredData = this.data;
      } catch (error) {
        console.error(error);
      }
    },
    filterData() {
      if (!this.vinSearch) {
        return (this.filteredData = this.data);
      }
      return (this.filteredData = this.data.filter((car) =>
        car.vin.toString().includes(this.vinSearch.toString())
      ));
    },
    onPageChange(event) {
      this.currentPage = event.page + 1;
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<style lang="scss" scoped>
.header {
  margin-left: 288px;
  margin-right: 32px;
}
.header-button {
  width: 42px;
  height: 42px;
}
.profile-pic {
  width: 46px;
  height: 46px;
}
.car-preview {
  width: 100%;
  border-radius: 8px;
}
.checked {
  background-color: #e4f5dd !important;
  color: #7fc75e;
}
.red-button {
  background: #d90e32;
}
.red-border {
  border: 1px solid#D90E32;
}
.border-05 {
  border: 0.5px solid var(--surface-300);
}
</style>
