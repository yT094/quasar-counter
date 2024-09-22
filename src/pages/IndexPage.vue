<template>
  <q-page
    class="flex flex-center text-white"
    v-touch-pan.prevent.mouse="handlePan"
  >
    <div class="row">
      <q-input
        v-model="data.name"
        input-class="text-center text-h5 text-white"
        color="teal"
        filled
        placeholder="Counter"
      />
    </div>
    <div class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          v-touch-repeat:300:300:300:300:50.mouse="decreaseCounter"
          size="xl"
          round
          color="primary"
          icon="remove"
          @click="decreaseCounter"
        />
      </div>
      <div class="col text-center text-h2">{{ data.counter }}</div>
      <div class="col text-center">
        <q-btn
          v-touch-repeat:300:300:300:300:50.mouse="increaseCounter"
          size="xl"
          round
          color="primary"
          icon="add"
          @click="increaseCounter"
        />
      </div>
    </div>
    <div class="row">
      <q-btn
        size="xl"
        round
        color="primary"
        icon="restart_alt"
        @click="resetCounter"
      />
    </div>
  </q-page>
</template>

<script setup>
defineOptions({
  name: "IndexPage",
});

/**
 * import
 */
import { reactive, watch } from "vue";
import { useQuasar } from "quasar";

/**
 * quasar
 */
const $q = useQuasar();

/**
 * data
 */
const data = reactive({
  counter: 0,
  name: "",
});

const savedData = $q.localStorage.getItem("data");
if (savedData) Object.assign(data, savedData);

watch(data, (value) => {
  $q.localStorage.set("data", value);
});

/**
 * counter methods
 */
const increaseCounter = () => {
  data.counter++;
};

const decreaseCounter = () => {
  if (data.counter > 0) {
    data.counter--;
  }
};

const resetCounter = () => {
  data.counter = 0;
  data.name = "";
};

const handlePan = (e) => {
  if (e.delta.y < 0) increaseCounter();
  else decreaseCounter();
};
</script>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
}
</style>
