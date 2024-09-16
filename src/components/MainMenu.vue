<template>
  <div class="navbar">
    <div class="flex flex-column fixed">
      <div class="mt-4 text-center dm-sans-700 text-100 text-2xl">
        DEMO TEST
      </div>

      <div class="mt-8 dm-sans-700">
        <div class="flex align-items-center menu-item">
          <img class="icon" src="../assets/images/user1.svg" alt="" />
          <div class="pl-3 text-100">Profile</div>
        </div>
        <div class="flex align-items-center menu-item active">
          <img class="icon" src="../assets/images/rim1.svg" alt="" />
          <div class="pl-3 text-100">Vehicles</div>
        </div>
        <div class="flex align-items-center menu-item">
          <img class="icon" src="../assets/images/settings1.svg" alt="" />
          <div class="pl-3 text-100">Settings</div>
        </div>
      </div>
    </div>
    <img class="mt-auto wave-image" src="../assets/images/wave.svg" alt="" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      menuItems: [
        { title: "Profile", link: "profile" },
        { title: "Vechicles", link: "Vechicles" },
        { title: "Settings", link: "Settings" },
      ],
    };
  },
  computed: {},
  methods: {},
};
</script>

<style lang="scss" scoped>
.navbar {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  width: 256px;
  background-color: #282828;
}
.menu-item {
  padding: 12px 30px;
  width: 256px;
  opacity: 0.45;
}
.icon {
  width: 24px;
  height: 25px;
  opacity: 100 !important;
}
.active {
  background-color: #535353;
  border-left: 2px solid #d90e32;
  opacity: 1;
}
.wave-image {
  width: 256px;
  height: 235px;
}
</style>
