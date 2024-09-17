<template>
  <main-menu></main-menu>
  <header-component :totalCars="totalCars"></header-component>

  <!-- серчбар и дропдаун для пагинатора -->
  <div class="container default-layout">
    <div class="grid mt-4">
      <div class="col-12 sm:col-6 lg:col-4">
        <p-inputtext
          type="number"
          class="w-full h-full"
          placeholder="Search VIN"
          v-model="vinSearch"
          @input="debouncedSearch"
        >
        </p-inputtext>
      </div>
      <div class="col-12 sm:col-6 lg:col-4">
        <div class="flex">
          <div class="dm-sans-500 mr-1">Select vehicles per page:</div>
          <p-dropdown
            v-model="rowsPerPage"
            :options="rowsOptions"
            class="max-w-6rem h-auto md:w-14rem"
            @change="fetchData"
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
        v-for="car in data"
        :key="car.id"
      >
        <div class="p-3 grey-background border-round-lg">
          <div class="flex mb-1">
            <img class="ml-auto" src="./assets/images/more_horizontal.svg" />
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
        Showing {{ data.length }} out of {{ totalCars }}
      </div>

      <!-- пагинатор -->
      <p-paginator
        class="ml-auto"
        :rows="rowsPerPage"
        :totalRecords="totalCars"
        :first="(currentPage - 1) * rowsPerPage"
        @page="currentPage = $event.page + 1"
      ></p-paginator>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Dropdown from "primevue/dropdown";
import InputText from "primevue/inputtext";
import Button from "primevue/button";
import Paginator from "primevue/paginator";
import debounce from "lodash/debounce.js";
import MainMenu from "./components/MainMenu.vue";
import HeaderComponent from "./components/HeaderComponent.vue";

export default {
  components: {
    "p-dropdown": Dropdown,
    "p-button": Button,
    "p-inputtext": InputText,
    "p-paginator": Paginator,
    MainMenu,
    HeaderComponent,
  },
  data() {
    return {
      data: [],
      vinSearch: null,
      rowsPerPage: 9,
      currentPage: 1,
      rowsOptions: [9, 20],
      totalCars: 0,
      debounceTimer: null,
    };
  },
  watch: {
    currentPage() {
      this.fetchData();
    },
    vinSearch() {
      this.currentPage = 1;
      this.debouncedSearch();
    },
    rowsPerPage() {
      this.currentPage = 1;
      this.fetchData();
    },
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get(
          "https://api.caiman-app.de/api/cars-test",
          {
            params: {
              search: this.vinSearch || "",
              per_page: this.rowsPerPage,
              page: this.currentPage,
            },
          }
        );
        this.data = response.data.data;
        this.totalCars = response.data.meta.total;
      } catch (error) {
        console.error(error);
      }
    },
    debouncedSearch: debounce(function () {
      this.fetchData();
    }, 500),
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.header {
  margin-left: 288px;
  margin-right: 32px;
}
.car-count-sticker {
  padding: 8px 16px;
  margin-left: 18px;
  border-radius: 6px;
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
</style>
