  <template>
  <div v-if="!load" id="chart">
    <apexchart
      type="line"
      ref="chart"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
  <div v-else>Loading...</div>
</template>

<script>
export default {
  name: "Chart",
  props: ['chartOptions', 'series', 'load']
};
</script>

<style>

</style>
