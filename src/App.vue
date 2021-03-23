<template>
  <div id="app">
    <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
     <div class="container">

        <section>
          <div class="flex">

            <div class="max-w-xs p-4">
              <label for="wallet" class="block text-sm font-medium text-gray-700">Валютная пара</label>                
                
                <div class="mt-1 relative rounded-md shadow-md">
                  <input
                    v-model="ticker1"
                    @keydown.enter="addTiсkerPair();"
                    @input.prevent="autoPasteTicker1(ticker1)"
                    @dblclick.prevent="ticker1=''"
                    type="text"
                    name="wallet"
                    id="wallet1"
                    class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                    placeholder="Например ETH"
                  />
                </div>
                
                <template v-if="ticker1Auto.length>0">
                  <div class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap">
                    <span
                      v-for="(t,index) in ticker1Auto"
                      :key="index"
                      @click.prevent="ticker1=t;ticker1Auto=[]"
                      class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer">
                      {{t}}
                    </span>
                  </div>
                </template>           

            </div>

            <div class="max-w-xs p-4">
              <label for="wallet" class="block text-sm font-medium text-gray-700">Валютная пара</label>                
                
                <div class="mt-1 relative rounded-md shadow-md">
                  <input
                    v-model="ticker2"
                    @keydown.enter="addTiсkerPair();"
                    @input.prevent="autoPasteTicker2(ticker2)"
                    @dblclick.prevent="ticker2='';tickerAlready = false;"
                    type="text"
                    name="wallet"
                    id="wallet2"
                    class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                    placeholder="Например ETH"
                  />
                </div>
                
                <template v-if="ticker2Auto.length>0">
                  <div class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap">
                    <span
                      v-for="(t,index) in ticker2Auto"
                      :key="index"
                      @click.prevent="ticker2=t;ticker2Auto=[]"
                      class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer">
                      {{t}}
                    </span>
                  </div>
                </template>           

            </div>

            <div v-if="tickerAlready" class="max-w-xs p-4">
              <div class="text-sm text-red-600">Такой тикер уже добавлен</div>
            </div>

          </div>
          

          <div class="flex">
            <div class="max-w-xs p-4">
              <button
                type="button"
                @click.prevent="addTiсkerPair()"
                class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                >
                <!-- Heroicon name: solid/mail -->
                <svg
                   class="-ml-0.5 mr-2 h-6 w-6"
                   xmlns="http://www.w3.org/2000/svg"
                   width="30"
                   height="30"
                   viewBox="0 0 24 24"
                   fill="#ffffff"
                   >
                   <path
                      d="M13 7h-2v4H7v2h4v4h2v-4h4v-2h-4V7zm-1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"
                      ></path>
                </svg>
                Добавить
              </button>
            </div>
          </div>



        </section>

        <hr class="w-full border-t border-gray-600 my-4" />

        <template v-if="tickers">
          <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">

            <div 
              class="bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid cursor-pointer"
              v-for="(t,index) in tickers"
              :key="index"
              @click="selectTicker(t)"
              :class="{
                'border-4': sel === t
              }"
            >
              <div class="px-4 py-5 sm:p-6 text-center">
                 <dt class="text-sm font-medium text-gray-500 truncate">
                   {{t.ticker1}} - {{t.ticker2}} <span class="bg-green-500" v-if="t.robotActive">***</span>
                 </dt>
                 <dd class="mt-1 text-3xl font-semibold text-gray-900">
                    {{t.price}}
                 </dd>
              </div>
              <div class="w-full border-t border-gray-200"></div>
              <button
                @click.stop="deleteTickerPair(t)"
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
                       ></path>
                  </svg>
                  Удалить
              </button>
            </div>

          </dl>
        </template>

      
        
        <template v-if="sel">
          <hr class="w-full border-t border-gray-600 my-4" />

          <div class="text-center">
            
            <dd class="mt-1 text-3xl font-semibold text-gray-900">
              <h2>{{sel.ticker1}} - {{sel.ticker2}}</h2>
            </dd>

            <template v-if="ordersActive">             

              <div class="text-center"> 

                <div v-for="order in ordersActive"
                  :key="order.id">

                    <template v-if="order.pair == sel.tickerPair">
                      <hr>
                      <dd class="mt-1 text-2xl font-semibold text-gray-900">
                       <b>{{order.pair}}({{order.type}})</b> - <b>#</b> {{order.order_id}} - <b>цена:</b> {{order.price}} - <b>кол-во:</b> {{order.amount}} -
                        <button
                          @click="orderDelete(order.order_id)"
                          type="button"
                          class="p-2 my-2 mr-5 bg-red-500 hover:bg-red-600 hover:shadow-lg text-white font-bold py-2 px-6 rounded-lg">
                        удалить
                      </button>
                      </dd>
                    </template>                 

                </div>

              </div>

            </template>

            <hr class="w-full border-t border-gray-600 my-4" />

            <template>

              <div class="flex">
                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Количество ({{sel.ticker1}})</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateBuyCount"
                        v-on:input="orderCreateBuyPriceAll = (orderCreateBuyCount * orderCreateBuyPrice)"
                        type="text"
                        name="wallet"                        
                        placeholder="0"
                         />
                   </div>
                </div>
                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Цена ({{sel.ticker2}})</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateBuyPrice"
                        v-on:input="orderCreateBuyCount = ((orderCreateBuyPriceAll / orderCreateBuyPrice).toFixed(4))"                       
                        type="text"
                        name="wallet"                        
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                         />
                   </div>
                </div>
                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Итого ({{sel.ticker2}})</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateBuyPriceAll"
                        v-on:input="orderCreateBuyCount = ((orderCreateBuyPriceAll / orderCreateBuyPrice).toFixed(4))"
                        type="text"
                        name="wallet"
                        placeholder="0"
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                         />
                   </div>
                </div>

                <button
                  @click="orderCreateFetch(sel.tickerPair, orderCreateBuyCount, orderCreateBuyPrice, 'buy')"
                  type="button"
                  class="p-2 my-2 mr-5 bg-green-500 hover:bg-green-600 hover:shadow-lg text-white font-bold py-2 px-6 rounded-lg">
                  купить
                </button>
                
                <template v-if="balance[0]">
                  Доступно:
                  <span class="hover:underline"
                    @click="orderCreateBuyPriceAll=balance[0][sel.ticker2];orderCreateBuyCount = ((orderCreateBuyPriceAll / orderCreateBuyPrice).toFixed(4))"
                  >
                    {{balance[0][sel.ticker2]}}
                  </span>
                  {{sel.ticker2}}
                </template>
              </div>

              <div class="flex">
                
                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Количество ({{sel.ticker1}})</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateSellCount"
                        v-on:input="orderCreateSellPriceAll = (orderCreateSellCount * orderCreateSellPrice)"
                        type="text"
                        name="wallet"
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                        placeholder="0"
                         />
                   </div>
                </div>

                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Цена</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateSellPrice"
                        v-on:input="orderCreateSellPriceAll = (orderCreateSellCount * orderCreateSellPrice)" 
                        type="text"
                        name="wallet"                        
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                         />
                   </div>
                </div>

                <div class="max-w-xs p-4">
                   <label for="wallet" class="block text-sm font-medium text-gray-700">Итого</label>
                   <div class="mt-1 relative rounded-md shadow-md">
                      <input
                        v-model="orderCreateSellPriceAll"
                        v-on:input="orderCreateSellCount = ((orderCreateSellPriceAll / orderCreateSellPrice).toFixed(4))"
                        type="text"
                        name="wallet"
                        placeholder="0"
                        class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                         />
                   </div>
                </div>

                 <button
                  @click="orderCreateFetch(sel.tickerPair, orderCreateSellCount, orderCreateSellPrice, 'sell')"
                  type="button"
                  class="p-2 my-2 mr-5 bg-red-500 hover:bg-red-600 hover:shadow-lg text-white font-bold py-2 px-6 rounded-lg">
                  продать
                </button>

                <template v-if="balance[0]">
                  Доступно:
                  <span class="hover:underline"
                    @click="orderCreateSellCount=balance[0][sel.ticker1];orderCreateSellPriceAll = (orderCreateSellCount * orderCreateSellPrice)"
                  >
                    {{balance[0][sel.ticker1]}}
                  </span>
                  {{sel.ticker1}}
                </template>

              </div>
              

            </template>

          </div>

        </template>

        <section v-if="sel" class="relative">
          <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
            {{ sel.tickerPair }}
          </h3>
          <div
            class="flex items-end border-gray-600 border-b border-l h-64"
            ref="graph"
          >
            <div
              v-for="(bar, idx) in normalizeGraph()"
              :key="idx"
              :style="{ height: `${bar}%` }"
              class="bg-purple-800 border w-10"
            ></div>
          </div>
          <button
            @click="sel = null"
            type="button"
            class="absolute top-0 right-0"
          >
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
              style="enable-background:new 0 0 512 512"
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
  </div>
</template>

<script>
//****    НИШТЯКИ ***** 
// При двойном нажатии на текстовом поле ввода тикера происходит сброс поля
// При вводе происходит подгрузка доступных пар, можно нажать, поле заполнится
// график в ширину при изменении ширины окна будет подстаиваться путём удаления элементов
// при создании ордера, можно нажать на количество в ДОСТУПНО: и произойдёт магия добавления
// добавление торговых пар работает и без ключей. Так как опрашивает api через fetch - можно написать функцию проверки apiKey и SecretKey - которая отправляет любой сокет запрос, а потом закрывает его в случае успеха

//[] 1. При добавлении НОВОЙ пары происходит создание нового WS public (subscribeTickerWS()), при монтировании норм // Критичность: 3
//[] 2. Пинг сервера в 10 минут, после которого без добавления новых ордеров отваливается WS активных ордеров // Критичность: 5+
//[] 3. Нет сообщений об ошибках, если не верный ключ или сигнатура. Обычный alert решит проблему, когда event="error" // Критичность: 3
//[] 4. Ширина столбца задана в коде, лучше вычислять через ref // Критичность: 1
//[] 5. При изменении ширины окна, количество элементов графика пересчитается ТОЛЬКО после того, как придёт обновление цены // Критичность: 1
//[] 6. Огромный css tailwind  - надо настроить и в prod будет очень красиво // Критичность: 1
//[] 7. Не реализован торговый бот, для чего это и писалось. Не нравятся существующие стратегии усреднения, можно уйти в минус, а для пар btc/ltc уж очень большая комиссия у биржи
//[] 8. Ключи хранятся в этом же файле, и доступны через this - по канонам надо в отдельный файл // Критичность: 4+ 

export default {
  name: 'App',
  data() {
    return {
      ticker1:'ETH',
      ticker2:'USDT',
      tickers:[],

      sel: null, //выбранная пара тикеров

      ordersActive:[],

      connection: null,
      priceM:'',

      apiKey:     'K-', // вставьте тут apikey биржи EXMO
      secretKey:  'S-', // вставьте тут secretKey биржи EXMO

      orderCreateBuyCount:'',
      orderCreateBuyPrice:'',
      orderCreateBuyPriceAll:'',

      orderCreateSellCount:'',
      orderCreateSellPrice:'',
      orderCreateSellPriceAll:'',

      graph: [],  // массив с данными цены для графика

      balance:[],
      pairTrue:[], // доступные пары тикеров


      ticker1Auto:[],
      ticker2Auto:[],
      tickerTrue:[], // доступные тикеры


      tickerAlready: false, // добавлен ли уже тикер

      maxGraphElements: 1, // максимальная щирина дом-элемента графика
    };
  },

  components: {

  },

  mounted() {   
    this.activeStatus()
    this.checkPairTrue()
    this.checkTickerTrue()

    window.addEventListener("resize", this.calculateMaxGraphElements);
  },

  

  created() {

    /////////
    const tickersList = localStorage.getItem("exmo-list-tickers");    
    this.tickers = JSON.parse(tickersList);
    /////////

     this.subscribeOrderWS()
     this.subscribeBalanceWS()
    
  },

  methods: {

    calculateMaxGraphElements() {

      if (!this.$refs.graph) {
        return;
      }

      this.maxGraphElements = (this.$refs.graph.clientWidth / 38); // коряво, надо ref вычислить ширину графика

    },

    normalizeGraph() {
      const maxValue = Math.max(...this.graph);
      const minValue = Math.min(...this.graph);
      return this.graph.map(
        price => 5 + ((price - minValue) * 95) / (maxValue - minValue)
      );
    },

    autoPasteTicker1(ticker) { // автозаполнение первого тикера, решил разделить, так как не охото условия городить, а тут красиво два метода.
      
      ticker = ticker.toUpperCase()
      this.ticker1Auto = []
      this.tickerAlready = false;   

      this.tickerTrue.forEach(t => {
        
        if ( t.indexOf(ticker) >=0 ) {
          this.ticker1Auto.push(t)         
        }

      }); 

    },

    autoPasteTicker2(ticker) { // автозаполнение второго тикера, решил разделить, так как не охото условия городить, а тут красиво два метода.
      
      ticker = ticker.toUpperCase()
      this.ticker2Auto = []
      this.tickerAlready = false;   

      this.tickerTrue.forEach(t => {
        
        if ( t.indexOf(ticker) >=0 ) {
          this.ticker2Auto.push(t)         
        }

      }); 

    },

    async checkTickerTrue() { // тикеры доступные для добавления - для быстрого выбора при вводе

      var jsonPair;

      await fetch("https://api.exmo.com/v1.1/currency")
        .then(response => response.text())
        .then(result => {
          jsonPair = result;
        })
        .catch(error => console.log('error', error));

        this.tickerTrue = JSON.parse(jsonPair);
      
    },

    async checkPairTrue() { // пары доступные для создания - для проверки при добавлении ПАРЫ

      await fetch("https://api.exmo.com/v1.1/pair_settings")
        .then(response => response.text())
        .then(result => {
          jsonPair = result;
        })
        .catch(error => console.log('error', error));

        var jsonPair = JSON.parse(jsonPair);
      
        for (let key in jsonPair) {
          this.pairTrue.push(key);
        }        

    },

    addTickerOfOrder(tickerOfPair) { // добавление при старте пар, в которых есть ордера, но которые отсутствуют на панели

      if (this.tickers) {
          if (this.tickers.find(t => t.tickerPair == tickerOfPair)) { //смотрим есть ли здесь аналогичный тикер 
           // ПОТОМ ПОДПРАВИТЬ не очень красиво     
          } else {

            const arrPair = tickerOfPair.split('_'); //разделим пару для тикера

            const newTickerPair = {
              ticker1: arrPair[0],
              ticker2: arrPair[1],
              tickerPair: tickerOfPair,
              price: '-',
              robotActive: true,
            }
            this.tickers.push(newTickerPair);
          }
        }
    },

    activeStatus() { // активность ордера на панели тикера

      if(this.tickers) {
        this.tickers.forEach(ticker => { ticker.robotActive = false; }); // замена в массиве всех полей robotActive

        this.ordersActive.forEach(order => {
            this.tickers.forEach(ticker => {

              if (ticker.tickerPair == order.pair) {
                ticker.robotActive = true;
              } 

            });        
          });
      } 

    },

    subscribeTickerWS() { // api ws.js

      ///// api ws.js
      console.log("Starting connection to (priceTicker) WebSocket Server")
      var connection = new WebSocket("wss://ws-api.exmo.com:443/v1/public")

      var pairWS = []

      if(this.tickers) {
        this.tickers.forEach(ticker => {
          const pairText = 'spot/ticker:'+ticker.tickerPair      
          pairWS.push(pairText);
        });
      }   

      const message = JSON.stringify({"id":1,"method":"subscribe","topics":pairWS})
      //console.log(message)

      connection.addEventListener('open', () => {
        connection.send(message);
      }, { once: true }
      )

      connection.addEventListener("message", e => {
        
        const messageContent = JSON.parse(e.data);

        if (messageContent.event === 'update')   { // обновление тикера

          const tickerPair = (messageContent.topic).replace('spot/ticker:', '')
          const tickerInfoWs = {
            tickerPair: tickerPair,
            price: messageContent.data.['last_trade']
          } 

          if (this.sel?.tickerPair === tickerInfoWs.tickerPair) {
            
            this.graph.push(tickerInfoWs.price);

            this.calculateMaxGraphElements()

             while (this.graph.length > this.maxGraphElements) {
              this.graph.shift();
            }            

          } 
          
          //this.tickers.find(t => t.tickerPair === tickerInfoWs.tickerPair).price = tickerInfoWs.price; // это вызывает ошибку если удаляем тикер

          if(this.tickers) {
            this.tickers.forEach(t => {
              if (t.tickerPair === tickerInfoWs.tickerPair) {
                t.price = tickerInfoWs.price
              }              
            }); 
          }
        }

      });

   
    },  // *api ws.js

    subscribeOrderWS() { // подписка на ордеры

      const messageLog = "Starting connection to (Активные ордера) WebSocket Server" // можно реализовать данную процедуру на все оставшиеся сокеты

      console.log(messageLog)
      var connection = new WebSocket("wss://ws-api.exmo.com:443/v1/private")

      const CryptoJS = require('crypto-js');      
      const nonce = Date.now();
      const sign =  CryptoJS.HmacSHA512(this.apiKey + nonce, this.secretKey).toString(CryptoJS.enc.Base64);      
      const data = JSON.stringify({"id":1,"method":"login","api_key":this.apiKey,"secret_key":this.secretKey,"nonce":nonce, "sign":sign})

      connection.addEventListener('open', () => {
        connection.send(data);
      }, 
      )

      const message = JSON.stringify({"id":2,"method":"subscribe","topics":["spot/orders","spot/user_trades"]})
      
      connection.addEventListener('open', () => {
        connection.send(message);
      }, 
      )

      //const indexSnapshot = 'snapshot';
      //const indexUpdate = 'update';


      connection.addEventListener("message", e => {        
        const messageContent = JSON.parse(e.data);

        if (messageContent.event === 'snapshot')   { // получение всех пар от сокета при первичном обращении

          for (var s in messageContent.data) {
            this.ordersActive.push(messageContent.data[s])
            this.addTickerOfOrder(messageContent.data[s]['pair'])
          } 

        }

        if (messageContent.event === 'update')   {          
        
          if(messageContent.data['status'] == 'open') {
            this.ordersActive.push(messageContent.data)
            this.addTickerOfOrder(messageContent.data['pair'])
            //console.log(messageContent.data['pair'])
          }

          if(messageContent.data['status'] == 'cancelled') { // ордер закрыт - чистим
            // this.tickers.find(t => t.pair === messageContent.data['order_id']).robotActive = false ///!!!!!!!!!!!!!!!!!!!!!!
            this.ordersActive = this.ordersActive.filter(o => o.order_id !== messageContent.data['order_id']);

          }
            
        }

        if (messageContent.event === 'error')   {
          alert(messageContent.message + ' -- subscribeOrderWS() - ' + messageLog)
        }
      });

    },

    async subscribeBalanceWS() { //подписка на баланс

      this.balance = [];

      console.log("Starting connection to (priceTicker) WebSocket Server")
      var connectionBalance = new WebSocket("wss://ws-api.exmo.com:443/v1/private")

      const CryptoJS = require('crypto-js');      
      const nonce = Date.now();
      const sign =  CryptoJS.HmacSHA512(this.apiKey + nonce, this.secretKey).toString(CryptoJS.enc.Base64);      
      const data = JSON.stringify({"id":2,"method":"login","api_key":this.apiKey,"secret_key":this.secretKey,"nonce":nonce, "sign":sign})

      connectionBalance.addEventListener('open', () => {
        connectionBalance.send(data);
      }, 
      )

      const message = JSON.stringify({"id":2,"method":"subscribe","topics":["spot/wallet"]})
      
      connectionBalance.addEventListener('open', () => {
        connectionBalance.send(message);
      }, 
      )


      connectionBalance.addEventListener("message", e => {        
        const messageContent = JSON.parse(e.data);
       
        if (messageContent.event === 'snapshot')   { 

          this.balance.push(messageContent.data['balances'])
          this.balance.push(messageContent.data['reserved'])
        
        }

        if (messageContent.event === 'update')   {
          connectionBalance.close()
          console.log('update получен')
          this.subscribeBalanceWS()
        }

      });

    },

    async orderCreateFetch(pair, quantity, price, typeOrder) { // создание ордера (купля или продажа)

      if (quantity <= 0)  {
        console.log('количество равно 0')
        return; // ничего не будем делать
      }

      // погнали создавать ордер, это надо вытащить потом в отдельную апи, так как сменится сервис на socket и усё, аля-улю

      const CryptoJS = require('crypto-js');      

      const nonce = 16154447266826668 + Date.now();
      const nonce2 = 'nonce='+nonce+'&pair='+pair+'&quantity='+quantity+'&price='+price+'&type='+typeOrder;

      const sign =  CryptoJS.HmacSHA512(nonce2, this.secretKey).toString(CryptoJS.enc.hex);

      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");
      myHeaders.append("Key", this.apiKey);
      myHeaders.append("Sign", sign);

      var urlencoded = new URLSearchParams();
      urlencoded.append("nonce", nonce);
      urlencoded.append("pair", pair);
      urlencoded.append("quantity", quantity); 
      urlencoded.append("price", price); 
      urlencoded.append("type", typeOrder);       

      var requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: urlencoded,
        redirect: 'follow'
      };

      var orderRobotId = '';


     await fetch("https://api.exmo.com/v1.1/order_create", requestOptions)
        .then(response => response.text())
        .then(result => {
          orderRobotId = result;
        })
        .catch(error => console.log('error', error));

      let json = JSON.parse(orderRobotId);
      orderRobotId = (json['order_id'])

      if (json['result'] == true) {
        console.log('сработало')
      } else {
        alert(json['error']);
      }
    
    },

    addTiсkerPair() {

      const tickerPair = this.ticker1.toUpperCase() + '_' + this.ticker2.toUpperCase();

      if (this.pairTrue.find(p => p == tickerPair)) { //смотрим есть ли здесь аналогичный тикер        
      
        const newTickerPair = {
          ticker1: this.ticker1.toUpperCase(),
          ticker2: this.ticker2.toUpperCase(),
          tickerPair: tickerPair,
          price: '-',
          robotActive: false,
        } 

        if (this.tickers) {    

          if (this.tickers.find(t => t.tickerPair == tickerPair)) { //смотрим есть ли здесь аналогичный тикер
            this.tickerAlready = true;   
            return;
          }
        }

        this.tickers = this.tickers || [];

        this.tickers.push(newTickerPair);     
        

        //localStorage.setItem('exmo-list-tickers', JSON.stringify(this.tickers)); // пашет так как в localStorage записывается из watch

      } else {
        alert('Такой пары не существует!');
      }

    },

    selectTicker(ticker) {

      this.sel = ticker;

      this.graph = [];
      this.graph.push(ticker.price);

      this.orderCreateBuyPrice = ticker.price
      this.orderCreateSellPrice = ticker.price

      this.orderCreateBuyCount = ''
      this.orderCreateBuyPriceAll = ''

      this.orderCreateSellCount= ''
      this.orderCreateSellPriceAll= ''



    },

    deleteTickerPair(tickerToRemove) {

      this.tickers = this.tickers.filter(t => t.tickerPair !== tickerToRemove.tickerPair);
      this.sel=''

    },

    orderDelete(order_id) {      

      const CryptoJS = require('crypto-js');      

      const nonce = 16154447266826668 + Date.now();
      const nonce2 = 'nonce='+nonce+'&order_id='+order_id;

      const sign =  CryptoJS.HmacSHA512(nonce2, this.secretKey).toString(CryptoJS.enc.hex);

      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");
      myHeaders.append("Key", this.apiKey);
      myHeaders.append("Sign", sign);

      var urlencoded = new URLSearchParams();
      urlencoded.append("nonce", nonce);
      urlencoded.append("order_id", order_id);      

      var requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: urlencoded,
        redirect: 'follow'
      };


      fetch("https://api.exmo.com/v1.1/order_cancel", requestOptions);   

    },

   
  },

  watch: {

    tickers() {

      this.subscribeTickerWS()

      localStorage.setItem("exmo-list-tickers", JSON.stringify(this.tickers));

      this.activeStatus()     

    },

    ordersActive() {
      this.activeStatus() 
    },

  }

}
</script>

<style src='./app.css'>

</style>