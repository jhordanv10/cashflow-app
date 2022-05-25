<template>
  <main>
      <p>{{ labelVisual }}</p>
      <h1 :class="{ 
          'red': isNegative, 
          'green': !isNegative, 
        }">{{ amountCurrency }}</h1>
      <div class="graphic"> 
          <slot name="graphic"></slot>
      </div>
      <div class="action"> 
          <slot name="action"></slot>
      </div>
  </main>
</template>

<script>

const currencyFormatter = new Intl.NumberFormat("es-CO", {
    style: "currency",
    currency: "COP",
})

export default {
    props: {
        labelInitial: {
            type: String,
        },
        label: {
            type: String,
            default: null,
        },
        totalAmont: {
            Type: Number,
        },
        amount: {
            type: Number,
            default: null,
        }

    }, computed: {
        amountVisual() {
            return this.amount !== null ?  this.amount : this.totalAmont
        },
        labelVisual() {
            return this.amount !== null ?  this.label : this.labelInitial
        },
        amountCurrency() {
            return currencyFormatter.format(this.amountVisual)
        },
        isNegative() { 
            return this.amountVisual < 0 
        }

    }
}
</script>


<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-top: -120px;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 8px;
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 15px 24px;
  box-sizing: border-box;
}
.red {
  color: #C85C5C;
  font-weight: bold;
}
.green {
  color: var(--brand-green);
  font-weight: bold;
}
</style>
