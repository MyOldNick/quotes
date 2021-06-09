<template>
  <v-app class="app">
    <Header />
    <v-row class="app-row">
      <v-col md="4" xs="12">
        <CryptPanel
          :data="btc"
          :active="activeSymbol"
          :selectActive="selectActive"
        />
      </v-col>
      <v-col class="app-chart-col">
        <Chart :chartOptions="chartOptions" :series="series" :load="load" />
      </v-col>
      <PanelHeader class="app-panel-header"/>
    </v-row>
  </v-app>
</template>

<script>
import Header from "./components/Header";
import CryptPanel from "./components/CryptPanel";
import Chart from "./components/Chart";
import PanelHeader from "./components/PanelHeader"
//constants
import { API_KEY } from "./constants/API";

export default {
  name: "App",
  components: {
    Header,
    CryptPanel,
    Chart,
    PanelHeader
  },

  data: () => ({
    btc: [],
    activeSymbol: "BTC",
    load: true,
    series: [
      {
        name: "BTC",
        data: [],
      },
    ],

    chartOptions: {
      chart: {
        height: 350,
        type: "line",
        zoom: {
          enabled: false,
        },
        animations: {
          enabled: true,
          easing: "linear",
          dynamicAnimation: {
            speed: 1000,
          },
        },
      },
      dataLabels: {
        enabled: false,
      },
      stroke: {
        curve: "straight",
      },
      title: {
        text: "USD | BTS",
        align: "center",
      },
      grid: {
        row: {
          colors: ["#f3f3f3", "transparent"],
          opacity: 0.5,
        },
      },
      xaxis: {
        range: 10,
        categories: [new Date().toLocaleTimeString()],
      },
    },
  }),

  mounted() {
    this.getDataFromCharts();

    const ccStreamer = new WebSocket(
      "wss://streamer.cryptocompare.com/v2?api_key=" + API_KEY
    );
    ccStreamer.onopen = function onStreamOpen() {
      const subRequest = {
        action: "SubAdd",
        subs: [
          "2~Coinbase~BTC~USD",
          "2~Coinbase~ETH~USD",
          "2~Coinbase~BCH~USD",
          "2~Coinbase~XRP~USD",
        ],
      };
      ccStreamer.send(JSON.stringify(subRequest));
    };

    ccStreamer.onmessage = async (message) => {
      const res = await JSON.parse(message.data);

      if (res.PRICE) {

        const index = this.btc.findIndex((el) => el.FROMSYMBOL === res.FROMSYMBOL);

        if (index !== -1) {

          const percent = (res.PRICE * 100) / this.btc[index].PRICE - 100

          res.CHANGES = Math.round((percent) *100 ) / 100

          this.btc.splice(index, 1, res);

        } else this.btc.push(res);

        if (res.FROMSYMBOL === this.activeSymbol) {

          const seriesArr = [...this.series];

          seriesArr[0].data.push(res.PRICE);

          this.series = seriesArr;

          const obj = { ...this.chartOptions.xaxis };

          obj.categories.push(new Date(res.LASTUPDATE * 1000).toLocaleTimeString());

          this.chartOptions.xaxis = obj;
        }
      }
    };
  },

  methods: {
    selectActive(data) {

      this.load = true;

      this.activeSymbol = data;

      this.series[0].name = data;

      this.chartOptions.title.text = `USD | ${data}`;

      this.getDataFromCharts();
    },

    getDataFromCharts() {
      this.series[0].data = [];

      this.chartOptions.xaxis.categories = [];

      fetch(
        `https://min-api.cryptocompare.com/data/v2/histominute?fsym=${this.activeSymbol}&tsym=USD&limit=10&api_key=${API_KEY}`
      )
        .then((res) => res.json())
        .then(({ Data }) => {
          Data.Data.forEach((el) => {
            this.series[0].data.push(el.close);
            this.chartOptions.xaxis.categories.push(
              new Date(el.time * 1000).toLocaleTimeString()
            );
          });

          this.load = false;
        });
    },
  },
};
</script>

<style>
.app {
  margin: 0 0;
  padding: 0 0;
  box-sizing: border-box;
  overflow: hidden;
  background-color: black !important;
}

.app-panel-header {
    display: none !important;
    margin-top: 15px;
}

.app-chart-col {
  background-color: #eeeeee; 
  margin-top: 23px;
}


@media(max-width: 768px) {
  .app {
    overflow-y: auto;
  }
  .app-row {
    display: flex;
    flex-direction: column-reverse;
  }

  .app-panel-header {
    display: flex !important;
  }

  .app-chart-col { 
    margin-top: 5px;
}
}
</style>
