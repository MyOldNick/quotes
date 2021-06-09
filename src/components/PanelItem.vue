<template>
  <div @click="selectActive(fromsymbol)" :class="['panel-item-container', active === fromsymbol ? 'panel-item-container-active' : '' ]">
    <v-row class="panel-item-row">
      <v-col cols="5" class="panel-item-col-text">{{`${tosymbol} | ${fromsymbol}`}}</v-col>
      <v-col
        cols="4"
        class="panel-item-col-text d-flex justify-center">
        <p :class="['panel-changes', changes > 0 ? 'panel-changes-up' : 'panel-changes-down']">{{Math.abs(changes) || 0}}%</p>
      </v-col>
      <v-col
        cols="3"
        class="panel-item-col-text d-flex justify-end"
        ><p>{{price}}</p>
      </v-col>
    </v-row>
  </div>
</template>
<script>
export default {
  name: "PanelItem",
  props: ['price', 'changes', 'fromsymbol', 'tosymbol', 'active', 'selectActive'],

};
</script>
<style scoped>
.panel-item-container {
  box-sizing: border-box;
  max-height: 46px;
  user-select: none;
  cursor: pointer;
  
}

.panel-item-container-active {
    background: linear-gradient(90deg, rgba(31, 70, 106, 0.88) 0%, #191A20 138.3%), #191A20;
}

.panel-item-row {
  width: 95%;
  margin: 0 auto;
}

.panel-item-col-text {
  color: white
}

.panel-changes {
  width: 100%;
  display: flex;
  justify-content: center;
}

.panel-changes-up::before {
  position: absolute;
  display: block;
  width: 50px;
  height: 50px;
  margin-top: 3px;
  margin-right: 25px;
  content: '';
  background-image: url('../assets/arrow-up.png');
}

.panel-changes-down::before {
  position: absolute;
  display: block;
  width: 50px;
  height: 50px;
  margin-top: 3px;
  margin-right: 25px;
  content: '';
  background-image: url('../assets/arrow-down.png');
}
</style>