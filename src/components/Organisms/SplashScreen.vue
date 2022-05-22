<template>
  <div class="splashscreen">
    <img src="@/assets/app-icon.png" alt="Logo-app" />
    <h1>Welcome to <span> <br> Cashflow App</span></h1>
    <Loading/>
  </div>
</template>

<script setup>
  import Loading from "../Atoms/Loading.vue"
  
</script>

<style scoped>
.splashscreen {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
}
h1 {
  color: var(--brand-green);
  text-align: center;
  /* opacity: 0.4; */
}
h1 span {
  color: var(--brand-blue);
  
}
img {
  width: 130px;
  height: 130px;
  /* opacity: 0.4; */
}

</style>
