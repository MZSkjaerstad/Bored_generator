<template>
   <div class="bored">
      <div class="bored__app">
         <div class="bored__logo">
            <h2 class="bored__logo--small">
               Are you
            </h2>

            <h1 class="bored__logo--big">
               bored
            </h1>

            <h2 class="bored__logo--small bored__logo--last">
               are you?
            </h2>
         </div>

         <div class="bored__output">
            <p class="bored__key">Nr. {{ values.key }}</p>

            <p class="bored__activity">"{{ values.activity }}"</p>

            <p class="bored__link"><a :href="values.link"> {{ values.link }} </a></p>

            <p class="bored__type">{{ values.type }}</p>

            <div class="bored__details">
               <div class="bored__participants bored__details-column">
                  <p class="bored__details-title">Participants</p>

                  <p class="bored__details-value">{{values.participants}}</p>
               </div>

               <div class="bored__price bored__details-column">
                  <p class="bored__details-title">Price</p>

                  <p class="bored__details-value">{{values.price}}</p>
               </div>

               <div class="bored__accessibility bored__details-column">
                  <p class="bored__details-title">Accessibility</p>

                  <p class="bored__details-value">{{values.accessibility}}</p>
               </div>
            </div>
         </div>

         <div class="bored__controls">
            <button class="bored__generate bored__button" @click="getRandom">Generate</button>

            <button class="bored__filter bored__button" @click="toggleMenu">filter</button>

            <FilterMenu
               class="bored__filter-menu"
               v-if="menuToggled === true"
               :value="values"
               :toggleMenu="toggleMenu"
               :getSpecified="getSpecified"
               @input="(newValue) => {value = newValue}"
            />
         </div>
      </div>
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

   /* Fetch random dataset */
      async getRandom() {
         const url = 'https://www.boredapi.com/api/activity/';
         const response = await fetch(url);
         try {
            await this.handleRandomFetch(response)
         } catch(error) {
            this.error = error.message;
         }
      },

      async handleRandomFetch(response) {
         if (response.status >= 200 && response.status < 300) {
            const results = await response.json();

            this.values.key = results.key;
            this.values.type = results.type;
            this.values.activity = results.activity;
            this.values.link = results.link
            this.values.participants = results.participants;
            this.values.accessibility = results.accessibility;
            this.values.price = results.price;

            this.values.accessibility = this.filterAccess()
            this.values.price = this.filterPrice()

         } else {
            if(response.status === 404) {
               throw new Error('Url ikke funnet.');
               }
               if(response.status === 401) {
                  throw new Error('Ikke authorisert.');
               }
               if(response.status > 500) {
                  throw new Error('Server error.');
               }
            throw new Error('Her gikk noe galt.');
         }
      },

   /* Fetch dataset spesified by edited values in filter */
      async getSpecified() {
         const url = `https://www.boredapi.com/api/activity/?type=${this.values.type}&participants=${this.values.participants}`;
         const response = await fetch(url);
         try {
            await this.handleSpesifiedFetch(response);
         } catch(error) {
            this.error = error.message;
         };
      },

      async handleSpesifiedFetch(response) {
         if (response.status >= 200 && response.status < 300) {
            const results = await response.json();

            if (results.error === 'No activity found with the specified parameters') {
               this.values.activity = 'Sorry, we did not find a suitable activity'

            } else {
               this.values.key = results.key;
               this.values.type = results.type;
               this.values.activity = results.activity;
               this.values.link = results.link
               this.values.participants = results.participants;
            }

            this.values.accessibility = this.filterAccess()
            this.values.price = this.filterPrice()
            this.toggleMenu()

         } else {
            if(response.status === 404) {
               throw new Error('Url ikke funnet.');
               }
               if(response.status === 401) {
                  throw new Error('Ikke authorisert.');
               }
               if(response.status > 500) {
                  throw new Error('Server error.');
               }
               throw new Error('Her gikk noe galt.');
         }
      },

   /* Convert price in fetched dataset into a more readable value. */
      filterPrice() {
         let priceValue = this.values.price

         if (priceValue > 0.76 && priceValue < 1.00) {
            priceValue = '$$$$'
         } else if (priceValue > 0.51 && priceValue < 0.75) {
            priceValue = '$$$'
         } else if (priceValue > 0.26 && priceValue < 0.50) {
            priceValue = '$$'
         } else if (priceValue > 0.01 && priceValue < 0.25) {
            priceValue = '$'
         } else {
            priceValue = 'FREE'
         }
         return priceValue
      },

   /* Convert access in fetched dataset into a more readable value. */
      filterAccess() {
         let accessValue = this.values.accessibility

         if (accessValue > 0.76 && accessValue < 1.00) {
            accessValue = 'Very hard'
         } else if (accessValue > 0.51 && accessValue < 0.75) {
            accessValue = 'Hard'
         } else if (accessValue > 0.26 && accessValue < 0.50) {
            accessValue = 'Medium'
         } else if (accessValue > 0.01 && accessValue < 0.25) {
            accessValue = 'Easy'
         } else {
            accessValue = 'Very easy'
         }
         return accessValue
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
      padding: var(--spacing-medium);
      background: var(--secondary-color);
      display: flex;
      justify-content: center;
      align-items: center;
   }

   .bored__app {
      position: absolute;
      z-index: 10;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
   }

   .bored__logo {
      padding-bottom: var(--spacing-medium);
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
      font-size: var(--font-size-key);
      font-weight: 400;
   }

   .bored__activity {
      text-align: center;
      font-size: var(--font-size-activity);
      padding: var(--spacing-medium) 0 0 0;
   }

   .bored__link {
      height: 3rem;
      font-family: roboto;
      font-size: var(--font-size-link);;
      display: flex;
      align-items: center;
   }

   .bored__type {
      font-family: roboto;
      font-size: var(--font-size-type);
      font-weight: 600;
      text-transform: uppercase;
      padding: var(--spacing-padding) 0 var(--spacing-small) 0;
   }

   .bored__details {
      padding-top: var(--spacing-small);
      border-top: solid var(--highlight-color) 1px;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
   }

   .bored__details-column {
      padding: 0 var(--spacing-small);
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .bored__details-title {
      font-size: var(--font-size-key);
      font-family: roboto;
      padding-bottom: 0.5rem;
   }

   .bored__details-value {
      font-size: var(--font-size-value);
   }

   .bored__controls {
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .bored__generate {
      font-size: var(--font-size-value);
      margin: var(--spacing-large) 0 var(--spacing-small) 0;
      padding: var(--spacing-padding) var(--spacing-small);
   }

   .bored__filter {
      font-size: var(--font-size-key);
      padding: var(--spacing-padding);
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

   @media screen and (max-device-width: 767px) {
      .bored__link {
         height: 10rem;
      }
   }
</style>