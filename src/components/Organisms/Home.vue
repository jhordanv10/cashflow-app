<template>
  <Layout>
    <template #header>
      <Header />
    </template>

    <template #resume>
      <Resume
        :label="label"
        :labelInitial="'Ahorro total'"
        :totalAmont="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic 
            :amounts="amounts" 
            @select="select"
            />
        </template>
        <template #action>
          <Action @create="create" />
        </template>
      </Resume>
    </template>

    <template #movements>
      <Movements 
      :movements="movements" 
      @remove="remove"      
      />
    </template>
  </Layout>
</template>

<script>
import Layout from "../Molecules/Layout.vue";
import Header from "../Atoms/Header.vue";
import Resume from "../Atoms/Resume/Index.vue";
import Movements from "../Atoms/Movements/Index.vue";
import Action from "../Atoms/Resume/Action.vue";
import Graphic from "../Atoms/Resume/Graphic.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data() {
    return {
      amount: null,
      label: null,
      // amounts : [100,200,500,200,-400,-600,-300,0,300,500],
      movements: [],
    };
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);

          return m.time >= oldDate;
        })
        .map((m) => m.amount);

      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1);

        return lastMovements.reduce((sum, movement) => {
          return sum + movement;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce( (sum, mov) => {
        return sum + mov.amount
      }, 0)
    }
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"))

    if(Array.isArray(movements) ){
      this.movements =  movements?.map(m => {
        return {...m, time: new Date(m.time)}
      })
    }
  },
  methods: {
    create(movements) {
      this.movements.push(movements);
      this.save()
    },
    remove(id) {
      const index =  this.movements.findIndex( m => m.id === id)
      this.movements.splice(index, 1)
      this.save()
    }, 
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements))
    }, 
    select(el) {
      this.amount = el
    }, 
  },
};
</script>
