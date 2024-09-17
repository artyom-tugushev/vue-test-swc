<template>
  <div class="header align-items-center flex my-4">
    <div class="dm-sans-700 text-3xl">Vehichles</div>
    <div class="dm-sams-500 car-count-sticker grey-background">
      {{ totalCars }}
    </div>
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
          src="../assets/images/user_photo.jpg"
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
</template>

<script>
import Button from "primevue/button";
import Dropdown from "primevue/dropdown";

export default {
  components: {
    "p-dropdown": Dropdown,
    "p-button": Button,
  },
  props: {
    totalCars: {
      type: Number,
    },
  },
  data() {
    return {
      selectedLang: { lang: "En", code: "eng" },
      languages: [
        { lang: "En", code: "eng" },
        { lang: "Ru", code: "rus" },
      ],
    };
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
</style>
