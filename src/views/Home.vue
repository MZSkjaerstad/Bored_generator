<template>
   <div class="bored">
      <div class="bored__app">
         <div class="bored__logo">
            <div class="bored__logo--small">
               Are you
            </div>

            <div class="bored__logo--big">
               bored
            </div>

            <div class="bored__logo--small bored__logo--last">
               are you?
            </div>
         </div>

         <div class="bored__output">
            <div class="bored__key">Nr. {{ values.key }}</div>

            <div class="bored__activity">"{{ values.activity }}"</div>

            <div class="bored__link"><a :href="values.link"> {{ values.link }} </a></div>

            <div class="bored__type">{{ values.type }}</div>

            <div class="bored__details">
               <div class="bored__participants bored__details-column">
                  <div class="bored__details-title">Participants</div>

                  <div class="bored__details-value">{{values.participants}}</div>
               </div>

               <div class="bored__price bored__details-column">
                  <div class="bored__details-title">Price</div>

                  <div class="bored__details-value">{{values.price}}</div>
               </div>

               <div class="bored__accessibility bored__details-column">
                  <div class="bored__details-title">Accessibility</div>

                  <div class="bored__details-value">{{values.accessibility}}</div>
               </div>
            </div>
         </div>

         <div class="bored__controls">
            <button class="bored__generate bored__button" @click="getRandom">Generate</button>
            <button class="bored__filter bored__button" @click="toggleMenu">filter</button>
         </div>
      </div>

      <FilterMenu
         class="bored__filter-menu"
         v-if="menuToggled === true"
         :value="values"
         :toggleMenu="toggleMenu"
         :getSpecified="getSpecified"
         @input="(newValue) => {value = newValue}"
      />
   </div>
</template>

<script>
import FilterMenu from '../components/FilterMenu.vue'

export default {
   components: {
      FilterMenu
   },

   data() {
      return {
         values: {
            key: '0',
            activity: 'activity',
            link: '',
            type: 'type',
            participants: 0,
            accessibility: 0.0,
            price: 0,
         },
         menuToggled: false,
      }
   },

   methods: {
      async getRandom() {
         const url = 'http://www.boredapi.com/api/activity/';
         const res = await fetch(url);
         const results = await res.json();

         this.values.key = results.key;
         this.values.type = results.type;
         this.values.activity = results.activity;
         this.values.link = results.link
         this.values.participants = results.participants;
         this.values.accessibility = results.accessibility;
         this.values.price = results.price;

         console.log(results)
      },

      async getSpecified() {
         const url = `http://www.boredapi.com/api/activity/?type=${this.values.type}&participants=${this.values.participants}&price=${this.values.price}&accessibility=${this.values.accessibility}`;
         const res = await fetch(url);
         const results = await res.json();

         if (results.error === 'No activity found with the specified parameters') {
            this.values.activity = 'Sorry, we did not find a suitable activity'
            console.log(results)

         } else {
            this.values.key = results.key;
            this.values.type = results.type;
            this.values.activity = results.activity;
            this.values.link = results.link
            this.values.participants = results.participants;
            this.values.accessibility = results.accessibility;
            this.values.price = results.price;
            console.log(results)
         }
         this.toggleMenu()
      },

      toggleMenu() {
         this.menuToggled = !this.menuToggled;
      }
   },
}

</script>

<style>
   .bored {
      height: 100%;
      background: var(--secondary-color);
      display: flex;
      justify-content: center;
      align-items: center;
   }

   .bored__app {
      height: 35rem;
      position: absolute;
      z-index: 10;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
   }

   .bored__logo {
      width: 15rem;
   }

   .bored__logo--small {
      font-size: 1.6rem;
   }

   .bored__logo--big {
      color: var(--highlight-color);
      font-size: 4rem;
      font-weight: 700;
      text-transform: uppercase;
      font-family: bely-display, sans-serif;
      font-weight: 400;
      font-style: normal;
   }

   .bored__logo--last {
      text-align: end;
   }

   .bored__output {
      height: 45%;
      display: flex;
      flex-direction: column;
      justify-content: space-evenly;
      align-items: center;
   }

   .bored__key {
      font-family: roboto;
      font-size: 0.7rem;
      font-weight: 400;
   }

   .bored__activity {
      padding-top: 1rem;
      font-size: 1.4rem;
   }

   .bored__type {
      font-size: 0.85rem;
      text-transform: uppercase;
   }

   .bored__link {
      font-family: roboto;
      font-size: 0.8rem;
   }

   .bored__details {
      display: flex;
      padding-top: 2rem;
      border-top: solid var(--highlight-color) 1px;
   }

   .bored__details-column {
      padding: 0 2rem;
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .bored__details-title {
      font-size: 0.75rem;
      padding-bottom: 0.5rem;
   }

   .bored__details-value {
      font-size: 1.2rem;
   }

   .bored__controls {
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .bored__generate {
      width: 8rem;
      padding:  0.5rem;
      margin-bottom: 0.5rem;
   }

   .bored__filter {
      font-size: 0.6rem;
      width: 4rem;
      padding:  0.3rem;
   }

   .bored__button {
      color: var(--highlight-color);
      background: var(--secondary-color);
      border: solid var(--highlight-color) 1px;
      font-family: bely-display, sans-serif;
      font-weight: 400;
      font-style: normal;
      text-transform: uppercase;
   }

   .bored__button:hover {
      color: var(--secondary-color);
      background: var(--highlight-color);
   }

   .Bored__filter-menu {
      position: absolute;
      z-index: 20;
   }

</style>