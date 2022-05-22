<template>
  <header>
    <img
      class="header-logo"
      src="@/assets/app-icon.png"
      alt="Logo-app"
      @click="reload"
    />
    <h1>CashFlow</h1>
    <span></span>
  </header>
</template>

<script>
export default {
  methods: {
    reload() {
      location.reload();
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100vw;
}
img,
span {
  width: 48px;
  cursor: pointer;
}
h1 {
  font-size: 1.4rem;
  color: var(--brand-blue);
}

</style>
