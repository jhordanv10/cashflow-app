<template>
    <button @click="showModal = !showModal">Agregar movimiento</button>
    <teleport to=#app>
        <Modal 
            v-show="showModal"
            @closed="showModal = false"
            >
            <form @submit.prevent="submit">
                <div class="field">
                    <label >Título del movimiento</label>
                    <input type="text" v-model="title" >
                </div>
                <div class="field">
                    <label >Monto</label>
                    <input type="number" min="0" inputmode="numeric" pattern="[0-9]*" title="Non-negative integral number" v-model="amount">
                </div>
                <div class="field">
                    <label >Descripción</label>
                    <textarea rows="4" v-model="description" ></textarea>
                </div>

                <div class="field">
                    <label class="radio-label">
                        <input type="radio" v-model="movementType" value="Ingreso">
                        <span>Ingreso</span>
                    </label>
                    <label class="radio-label">
                        <input type="radio" v-model="movementType" value="Egreso">
                        <span>Egreso</span>
                    </label>
                </div>
                <div class="action">
                   <button>Guardar</button>
                </div>
            </form>
        </Modal>
    </teleport>
</template>

<script setup>
    import {ref, defineEmits} from "vue"
    import Modal from "../../Molecules/Modal.vue"

    const showModal =  ref(false)
    
    const title =  ref(" ")
    const amount =  ref(0)
    const description =  ref(" ")
    const movementType =  ref("Ingreso")

    const emit = defineEmits(["create"])

    const submit = () => {
        showModal.value = !showModal;
        emit("create", {
          title: title.value,
          description: description.value,
          amount: movementType.value === "Ingreso" ? amount.value : -amount.value,
          time: new Date(),
          id: new Date(),
        })
        title.value = " "
        description.value = " "
        amount.value = null
        movementType.value = "Ingreso"

    }

</script>



<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 10px 40px;
  border-radius: 60px;
  box-sizing: border-box;
  cursor: pointer;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>
