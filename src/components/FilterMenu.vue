<template>
   <div class="filter">
      <div class="filter__content">
         <p class="filter__title">Filter</p>

         <button class="filter__button filter__generate" @click="getSpecified">Generate</button>

         <p class="filter__subtitle">Activity</p>

         <select class="filter__activity" type="text" v-model="value.type">
            <option v-for="type in types" :value="type"> {{ type }} </option>
         </select>

         <p class="filter__subtitle">Partisipants</p>

         <input class="filter__participants" type="number" v-model="value.participants" >
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
      width: 100%;
      background: var(--secondary-color);
      padding: var(--spacing-small);
      margin-top: var(--spacing-medium);
      border-top: solid var(--highlight-color) 1px;
      display: flex;
      justify-content: center;
      align-items: center;
   }

   .filter__content {
      display: flex;
      height: 100%;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
   }

   .filter__subtitle {
      font-size: var(--font-size-key);
      font-family: roboto;
      margin-bottom: var(--spacing-padding) 0;
   }

   .filter__activity {
      width: 60%;
      text-align: center;
      font-size: var(--font-size-key);
      padding: var(--spacing-padding);
      border: solid var(--highlight-color) 1px;
      margin-bottom: var(--spacing-padding);
   }

   .filter__participants {
      width: 60%;
      text-align: center;
      font-size: var(--font-size-key);
      padding: var(--spacing-padding);
      border: solid var(--highlight-color) 1px;
   }

   .filter__button {
      color: var(--highlight-color);
      background: var(--secondary-color);
      border: solid var(--highlight-color) 1px;
      font-family: bely-display, sans-serif;
      font-weight: 400;
      font-style: normal;
      text-transform: uppercase;
   }

   .filter__button:hover {
      color: var(--secondary-color);
      background: var(--highlight-color);
   }

   .filter__generate {
      font-size: var(--font-size-value);
      padding: var(--spacing-padding) var(--spacing-small);
      margin: var(--spacing-small) 0 var(--spacing-padding) 0;
   }

   @media screen and (max-device-width: 767px) {
      .filter {
         height: 24rem;
         width: 50rem;
      }
   }
</style>