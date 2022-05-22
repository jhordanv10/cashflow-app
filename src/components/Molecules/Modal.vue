<template>
    <div class="modal">
        <div class="head">
            <!-- <p>Nuevo movimiento</p> -->
            <img @click="closed" class="imgClosed" src="@/assets/closed-icon.svg" alt="">
        </div>
        <div class="body">
            <slot></slot>
        </div>
    </div>
</template>

<script setup>
    import {defineEmits} from "vue"

    const emit = defineEmits("[closed]")

    const closed = () => emit("closed")
</script>


<style scoped>
.modal {
  z-index: 2;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: white;
}

.head {
  display: flex;
  justify-content:flex-end;
  align-items: center;
  width: 100%;
  padding: 24px 24px;
  box-sizing: border-box;
}
.imgClosed {
    width: 20px;
    height: 20px;
    cursor: pointer;
    text-align: right;
}
p {
  margin: 0;
  font-size: 1.5rem;
  color: var(--brand-blue);
}
</style>