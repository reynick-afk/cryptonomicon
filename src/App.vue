<template>
    <div class="container mx-auto flex flex-col items-center p-4">
        <!-- <div
              class="fixed w-100 h-100 opacity-80 bg-purple-800 inset-0 z-50 flex items-center justify-center"
            > -->
        <!-- <svg
                class="animate-spin -ml-1 mr-3 h-12 w-12 text-white"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
              >
                <circle
                  class="opacity-25"
                  cx="12"
                  cy="12"
                  r="10"
                  stroke="currentColor"
                  stroke-width="4"
                ></circle>
                <path
                  class="opacity-75"
                  fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                ></path>
              </svg> -->
        <!-- </div> -->
        <div class="container">
            <section>
                <div class="flex">
                    <div class="max-w-xs">
                        <label for="wallet" class="block text-sm font-medium text-gray-600">Тикер</label
                    >
                    <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="ticker"
                        v-on:keydown.enter="ticker && add()"
                        type="text"
                        name="wallet"
                        id="wallet"
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                        placeholder="Например DOGE"
                        @input="filterCoins"
                        @focus="filterCoins"
                        @blur="clearResults"
                      />
                    </div>
                    <div
                    class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
                    >
                          <ul>
                            <li v-for="(coin, index) in displayedCoins" :key="coin" :class="{ active: index === activeIndex }" @mouseenter="setActive(index)" @click="selectCoin(coin)" class="inline-flex items-center px-2 m-2 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer">
                              {{ coin }}
                            </li>
                          </ul>
                  
                    </div>
                    <div v-if="dublicat" class="text-sm text-red-600 inline-flex">
                      
                      Такой тикер уже добавлен

                        <div class="text-sm ml-4">
                      <button
                      @click="dublicat = false"
                      >
                      <svg fill="#000000" height="14px" width="14px" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
	                       viewBox="0 0 460.775 460.775" xml:space="preserve">
                        <path d="M285.08,230.397L456.218,59.27c6.076-6.077,6.076-15.911,0-21.986L423.511,4.565c-2.913-2.911-6.866-4.55-10.992-4.55
	                        c-4.127,0-8.08,1.639-10.993,4.55l-171.138,171.14L59.25,4.565c-2.913-2.911-6.866-4.55-10.993-4.55
	                        c-4.126,0-8.08,1.639-10.992,4.55L4.558,37.284c-6.077,6.075-6.077,15.909,0,21.986l171.138,171.128L4.575,401.505
	                        c-6.074,6.077-6.074,15.911,0,21.986l32.709,32.719c2.911,2.911,6.865,4.55,10.992,4.55c4.127,0,8.08-1.639,10.994-4.55
	                        l171.117-171.12l171.118,171.12c2.913,2.911,6.866,4.55,10.993,4.55c4.128,0,8.081-1.639,10.992-4.55l32.709-32.719
	                        c6.074-6.075,6.074-15.909,0-21.986L285.08,230.397z"/>
                    </svg>
                      
                      </button>
                      </div>
                    </div>               
                  </div>
                </div>
                <button
                  @click="ticker && add()"
                  type="button"
                  class="my-4 mr-2 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                >
                 
                  
                  Добавить
                </button>

                <button
                  @click="clearAllTickers"
                  type="button"
                  class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-offset-2 focus:ring-gray-500"
                >
              
                  

                

                  Удалить все тикеры 
                </button>
                 

              </section>


              <template v-if="tickers.length">
                <hr class="w-full border-t border-gray-600 my-4" />
                <div>
                  <button 
                  class="my-4 mr-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                  v-if="page > 1"
                  @click= "page = page - 1"
                  >
                  Назад
                  </button>
                  
                  <button 
                  class="my-4 mr-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                  v-if="hasNextPage"
                  @click= "page = page + 1"
                  >
                  Вперед
                  </button>

                </div>
                <div class="flex">
                <span class="mr-4 font-bold">Фильтр:</span>  
                <input v-model="filter" @input="page = 1" type="text" id="wallet" class="block w-56 pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md" /> 
                </div>
                
                <div v-if="emptyTicker ">
                  
                   <span class='my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-offset-2 focus:ring-gray-500'>
                      Ticker not found
                    </span> 
                  
                  <hr class="w-full border-t border-gray-600 my-4" />
                   

                  </div>


              
           <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
                <div
                  v-for="t in filteredTickers()"
                  :key="t.name"
                  @click="select(t)" 
                  :class="{
                    'border-4': sel === t
                  }"
                  class="bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid cursor-pointer"
                >
                  <div class="px-4 py-5 sm:p-4 text-center">
                    <dt class="text-sm font-medium text-gray-500 truncate">
                      {{ t.name }} - USD
                    </dt>
                    <dd class="mt-1 text-3xl font-semibold text-gray-900">
                      {{ t.price }}
                    </dd>
                  </div>
                  <div class="w-full border-t border-gray-200"></div>
                  <button
                    @click.stop="handleDelete(t)"
                    class="flex items-center justify-center font-medium w-full bg-gray-100 px-4 py-4 sm:px-6 text-md text-gray-500 hover:text-gray-600 hover:bg-gray-200 hover:opacity-20 transition-all focus:outline-none"
                  >
                    <svg
                      class="h-5 w-5"
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 20 20"
                      fill="#718096"
                      aria-hidden="true"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                        clip-rule="evenodd"
                      ></path></svg
                    >Удалить
                  </button>
                </div>
              </dl>
              <hr class="w-full border-t border-gray-600 my-4" />
            </template>
              <section v-if="sel" class="relative">
                <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
                  {{ sel.name }} - USD
                </h3>
                <div class="flex items-end border-gray-600 border-b border-l h-64">
                  <div 
                  v-for="(bar, idx) in normalizeGraph()"
                  :key="idx"
                  :style=" { height: `${bar}%`}"
                  class="bg-purple-800 border w-10">
                </div>
                </div>
                <button @click="sel = null" type="button" class="absolute top-0 right-0">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    xmlns:svgjs="http://svgjs.com/svgjs"
                    version="1.1"
                    width="30"
                    height="30"
                    x="0"
                    y="0"
                    viewBox="0 0 511.76 511.76"
                    style="enable-background: new 0 0 512 512"
                    xml:space="preserve"
                  >
                    <g>
                      <path
                        d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
                        fill="#718096"
                        data-original="#000000"
                      ></path>
                    </g>
                  </svg>
                </button>
              </section>
            </div>
          </div>
</template>

<script>

import axios from 'axios';

export default {
    name: "App",
   components: {

  },
   data() {
        return {
            ticker: "",
            tickers: [],
            sel: null,
            graph: [],
            coins: [],
            dublicat: null,
            activeIndex: -1,
            page: 1,
            filter: "",
            hasNextPage: true,
            emptyTicker: true
        };
    },


    // GET FROM LOCALSTORAGE 
    
    created() {
        const tickersData = localStorage.getItem("cryptonomicon-list")
        if(tickersData) {
          this.tickers = JSON.parse(tickersData)
          this.tickers.forEach(ticker => {
            this.subscribeToUpdates(ticker.name);
            
          })
        }
    },

    // MOUNTED COINS-LIST FOR AUTOCOMPLATE

    mounted() {
    this.loadCoinList();
    this.notFoundTickers();
  },


    methods: {


    // FILTER COINS FROM TICKERS

      filteredTickers() {
        const start = (this.page -1) * 6
        const end = this.page * 6

       
        const filteredTickers = this.tickers.filter(ticker => ticker.name.toLowerCase().includes(this.filter.toLowerCase()));
        
        this.hasNextPage = filteredTickers.length > end;
        this.emptyTicker = filteredTickers.length < 1;

        return filteredTickers.slice(start, end)
      },


    // SUBSCRIBE TO UPDATE COINS

      subscribeToUpdates(tickerName) {

        const intervalId = setInterval(async () => {
        const f = await fetch(
          `https://min-api.cryptocompare.com/data/price?fsym=${tickerName}&tsyms=USD&api_key=7800286bd2fdd7a2bc054933d8c77b23beafa42d6d67460da8cfcb89033a16a5`
        );
        const data = await f.json();

        let foundTicker = this.tickers.find(function(t){
          return t.name === tickerName;
        });
        if(foundTicker){
        if(data.USD > 1){
          foundTicker.price = data.USD.toFixed(2);
        } else {
          foundTicker.price = data.USD.toPrecision(2);
        }
        } else{
          clearInterval(intervalId);
        }


        if (this.sel?.name === tickerName) {
          this.graph.push(data.USD);
        }
        
      }, 5000);
      this.ticker = "";
      },


    // ADD TICKER

   add() {
  

      // CART TICKER

      const currentTicker = {
        name: this.ticker.toUpperCase(),
        price: "-"
      };
      
     


    // DUBLICAT TICKER FOUND

      let dublicatTicker = this.tickers.find(function(t){
        return t.name === currentTicker.name;
        });
        if(dublicatTicker){
          this.dublicat = true;
        } else{ 


      this.tickers.push(currentTicker);
      this.filter = "";
      this.dublicat = false;
      }
  

    // LOCAL STORAGE

    localStorage.setItem('cryptonomicon-list', JSON.stringify(this.tickers));
    this.subscribeToUpdates(currentTicker.name);

      },

    
   // DELETE TICKER

        handleDelete(tickerToRemove) {
          this.tickers = this.tickers.filter(t => t !== tickerToRemove)
          this.sel = null
          localStorage.setItem('cryptonomicon-list', JSON.stringify(this.tickers));
        },

  // DELETE ALL TICKERS 

        clearAllTickers() {
          this.tickers = [];
          localStorage.setItem('cryptonomicon-list', JSON.stringify(this.tickers))
        },


  // NORMALIZE GRAPH
        
        normalizeGraph(){
          const maxValue = Math.max(...this.graph);
          const minValue = Math.min(...this.graph);
          return this.graph.map(price => 
          5 + ((price - minValue) * 95) / (maxValue - minValue)
          )
        },

  // SELECT TICKER

        select(ticker){
          this.sel = ticker;
          this.graph = [];
        },

  // AUTOCOMPLATE
  
  loadCoinList() {
      axios.get('https://min-api.cryptocompare.com/data/all/coinlist?Symbol')
        .then(response => {
          this.coins = Object.keys(response.data.Data);
        })
        .catch(error => {
          console.error(error);
        });
    },
    filterCoins() {
      this.activeIndex = -1; // Сброс активного индекса при фильтрации
    },
    setActive(index) {
      this.activeIndex = index;
    },
    selectCoin(coin) {
      this.ticker = coin;
      this.add();
    }
  },

computed: {
    displayedCoins() {
      return this.coins
        .filter(coin => coin.toLowerCase().includes(this.ticker.toLowerCase()))
        .slice(0, 6);
    }
  }


}
  </script>

  <style src="./app.css"></style>
