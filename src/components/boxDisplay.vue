<template>
  <div style="background: black">
    <div class="boxSize q-ma-xs">
      <div class="row">
        <div class="vl q-mt-md" v-show="sensorData[4] == 1"></div>
        <div class="vl2 q-mt-md" v-show="sensorData[4] == 0"></div>
      </div>
      <div class="row q-mt-xs">
        <div class="vh q-mx-xs" v-show="sensorData[0] == 1"></div>
        <div class="vh2 q-mx-xs" v-show="sensorData[0] == 0"></div>
        <div class="vh q-mr-xs" v-show="sensorData[1] == 1"></div>
        <div class="vh2 q-mr-xs" v-show="sensorData[1] == 0"></div>
        <div class="vh q-mr-xs" v-show="sensorData[2] == 1"></div>
        <div class="vh2 q-mr-xs" v-show="sensorData[2] == 0"></div>
        <div class="vh q-mr-xs" v-show="sensorData[3] == 1"></div>
        <div class="vh2 q-mr-xs" v-show="sensorData[3] == 0"></div>
      </div>
      <div class="q-mt-sm" align="center">{{ label }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
    },
    sensorData: {
      type: Array,
    },
  },
};
</script>

<style lang="scss" scoped>
.boxSize {
  height: 80px;
  width: 80px;
  background-color: rgba($color: #c4c4c4, $alpha: 0.25);
}
.vl {
  background-color: green;
  width: 5px;
  height: 15px;
  position: relative;
  left: 55px;
}
.vl2 {
  background-color: red;
  width: 5px;
  height: 15px;
  position: relative;
  left: 55px;
}
.vh {
  background-color: green;
  width: 15px;
  height: 5px;
}
.vh2 {
  background-color: red;
  width: 15px;
  height: 5px;
}
</style>
