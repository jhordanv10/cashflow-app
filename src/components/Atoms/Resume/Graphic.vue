<template>
  <div>
    <svg 
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 250 200"
    >
      <line 
        stroke="#000" 
        stroke-width="1.5" 
        x1="0" 
        :y1="zero" 
        x2="250" 
        :y2="zero" 
      />
      <polyline
        fill="none"
        stroke="#aedee2"
        stroke-width="4"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#da922a"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />
    </svg>
    
  </div>
</template>

<script setup>
import { computed, toRefs, ref, defineEmits, watch } from "vue";

const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});

const {amounts} = toRefs(props)

const amountToPixels = (amount) => {
  const min = Math.min(...amounts.value)
  const max = Math.max(...amounts.value)

  const amountAbs =  amount + Math.abs(min)
  const minmax = Math.abs(max) + Math.abs(min)

  
  return 200 - ((amountAbs*100) / minmax ) * 2;

}

const zero = computed ( () => {
  return amountToPixels(0)
})

const points = computed( () => {
  const total = amounts.value.length

  return amounts.value.reduce( (acum, amount, i) => {
    const x = (250 / total) * (i + 1)
    const y = amountToPixels(amount)
    return `${acum} ${x},${y}`
  }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`)
})

const showPointer = ref(false)

const pointer = ref(0)

const emit = defineEmits(["select"])

watch(pointer, (value) => {
  const index = Math.ceil((value / ( 250 / amounts.value.length )))
  if (index < 0 || index > amounts.value.length) return
  
  emit("select", amounts.value[index - 1])

})



const tap = ({target, touches }) => {
  showPointer.value = true
  const elementWidth =  target.getBoundingClientRect().width
  const elementX =  target.getBoundingClientRect().x
  const touchX = touches[0].clientX

  pointer.value =  ((touchX - elementX) * 250) / elementWidth
}


const untap = () => {
  showPointer.value = false
}


</script>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>
