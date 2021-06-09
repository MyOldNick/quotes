<template>
  <div class="panel">
    <PanelHeader class="panel-top" />
    <div class="panel-main">
      <v-row class="panel-row">
        <v-col cols="5" class="panel-col-text">Валютные пары</v-col>
        <v-col cols="4" class="panel-col-text d-flex justify-center"
          >Изменение</v-col
        >
        <v-col cols="3" class="panel-col-text d-flex justify-end"
          ><p>Цена</p>
        </v-col>
      </v-row>
      <div v-if="data.length > 0">
        <PanelItem
          v-for="(el, index) in data"
          :key="index"
          :price="el.PRICE"
          :fromsymbol="el.FROMSYMBOL"
          :tosymbol="el.TOSYMBOL"
          :changes="el.CHANGES"
          :active="active"
          :selectActive="selectActive"
        />
      </div>
      <div class="panel-loader d-flex justify-center align-center" v-else>
        <p>Loading...</p>
      </div>
    </div>
  </div>
</template>
<script>
import PanelItem from "./PanelItem";
import PanelHeader from "./PanelHeader";

export default {
  name: "CryptPanel",
  components: {
    PanelItem,
    PanelHeader,
  },
  props: ["data", "active", "selectActive"],
};
</script>
<style scoped>
.panel {
  height: 100%;
  margin-top: 10px;
}
.panel-main {
  height: 100%;
  margin-top: 3px;
  background-color: #191a20;
}

.panel-col-text {
  font-size: 16px;
  line-height: 150%;
  color: rgba(117, 118, 121, 1);
}

.panel-loader {
  height: 100%;
  color: white;
}

.panel-row {
  width: 95%;
  margin: 0 auto;
  max-height: 50px;
}

@media (max-width: 768px) {
  .panel {
    margin-top: -10px;
  }

  .panel-top {
    display: none;
  }
}
</style>