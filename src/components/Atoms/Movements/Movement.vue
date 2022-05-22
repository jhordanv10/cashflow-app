<template>
  <div class="movement">
    <div class="content">
      <p class="date">{{ time.toLocaleString() }}</p> 
      <h4>{{ title.toUpperCase() }} </h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
        <img class="imgDelete" src="@/assets/delete-icon.svg" alt="delete" @click="remove">
        <p :class="{ 
          'red': isNegative, 
          'green': !isNegative, 
        }"
        > 
          {{ amountCurrency }}
        </p >
    </div>
  </div>
</template>

<script setup>
import { toRefs, computed, defineEmits } from "vue";

const currencyFormatter = new Intl.NumberFormat("es-CO", {
    style: "currency",
    currency: "COP",
})

const props = defineProps({
  id: {
    type: String,
  },
  title: {
    type: String,
  },
  description: {
    type: String,
  },
  amount: {
    type: Number,
  },
  time: {
    type: Date,
  },
});

const { id, title, description, amount, time } = toRefs(props);

const amountCurrency = computed( 
  () => currencyFormatter.format(amount.value)
)


const emit = defineEmits(["remove"])

const remove = () => {
    emit("remove", id.value)
}

const isNegative = computed( () => amount.value < 0 )

</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
.imgDelete {
    width: 18px;
    height: 18px;
    cursor: pointer;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: #C85C5C;
  font-weight: bold;
}
.green {
  color: var(--brand-green);
  font-weight: bold;
}
.date {
  color: var(--brand-blue); 
  font-size: 13px;
  margin-bottom: 5px;
  font-weight: bold;
}
</style>
