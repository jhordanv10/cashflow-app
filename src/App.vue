<template>
  <Suspense>
    <template #default>
      <Home />
    </template>
    <template #fallback>
      <SplashScreen />
    </template>
  </Suspense>
</template>

<script>
import SplashScreen from "@/components/Organisms/SplashScreen.vue";
import { defineAsyncComponent } from "vue";

export default {
  components: {
    SplashScreen,
    Home: defineAsyncComponent(
      () =>
        new Promise((resolve) => {
          setTimeout(() => {
            resolve(import("@/components/Organisms/Home.vue"));
          }, 2000);
        })
    ),
  },
};
</script>

<style>
html,
body,
#app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  overflow: hidden;
}
* {
  --brand-green: #8cc49b;
  --brand-blue: #6cccd4;
  --brand-orange: #da922a;
}
</style>
