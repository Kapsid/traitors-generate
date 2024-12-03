<template>
  <img
      :src="image"
      alt="Player Face"
      class="w-20 h-20 rounded-full mb-4 border border-green-700"
  />
  <h2 class="text-lg font-bold">{{ name }}</h2>
  <p class="text-sm">Věk: {{ age }}</p>
  <p class="text-sm">Pohlaví: {{ gender }}</p>
  <p class="text-sm">Povolání: {{ occupation }}</p>
  <p class="text-red-400 font-bold mt-2" v-if="isTraitor">
    Zrádce
  </p>
  <p class="text-green-400 font-bold mt-2" v-else>
    Věrný
  </p>
</template>

<script>
export default {
  props: {
    age: Number,
    gender: String,
    image: String,
    isTraitor: Boolean,
    name: String,
    occupation: String,
  }
}

</script>