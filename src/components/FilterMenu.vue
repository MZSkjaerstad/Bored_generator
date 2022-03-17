<template>
   <div class="filter">
      <div class="filter__content">
         <div class="filter__title">Filter</div>
         <select type="text" v-model="value.type">
            <option v-for="type in types" :value="type"> {{ type }} </option>
         </select>

         <input type="number" v-model="value.participants">

         <form>
            <input type="radio" v-for="price in prices" v-model="value.price" v-bind:value="price">
         </form>

         <form>
            <input type="radio" v-for="index in accessibilities" v-model="value.accessibility" v-bind:value="index">
         </form>

         <button @click="getSpecified">Generate</button>
      </div>
   </div>
</template>

<script>
export default {
   emits: ["input"],
   props: {
      value: {
            type: Object,
            required: true
      },
      getSpecified: Function,
      toggleMenu: Function,
   },

   data() {
      return {
         types: ['education', 'recreational', 'social', 'diy', 'charity', 'cooking', 'relaxation', 'music', "busywork"],
         prices: [0, 0.2, 0.4, 0.6, 0.8, 1],
         accessibilities: [0, 0.2, 0.4, 0.6, 0.8, 1],
      }
   },

   watch: {
      value() {
            this.$emit('input', this.value);
      }
   }
}
</script>

<style >
   .filter {
      height: 80%;
      width: 40%;
      background: var(--secondary-color);
      border: solid var(--highlight-color) 1px;
      display: flex;
      justify-content: center;
      align-items: center;
   }

   .filter__content {
      width: 75%;
      height: 70%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
   }
</style>