<template>
  <div class="fullscreen">
    <div class="TitleBox fixed-center">
      <div class="row">
        <div>
          <img src="../../public/image/logo.svg" alt="" style="width: 280px" />
        </div>
        <div>
          <div class="fontTitle q-pt-xl">BHMS Analysis program</div>
          <div class="fontSub">Burapha Withi Express Health Monitor System</div>
        </div>
      </div>
    </div>
    <div class="fixed-bottom">
      <div align="center" class="q-pb-sm">Loading data from datalogger...</div>
      <div class="q-mb-md">
        <div class="OutsideBar">
          <div class="InsideBox" :style="{ width: progress + '%' }"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      progress: 0,
    };
  },

  mounted() {
    setTimeout(() => {
      this.progress = 25;
    }, 2000);
    setTimeout(() => {
      this.progress = 50;
    }, 4000);
    setTimeout(() => {
      this.progress = 75;
    }, 6000);
    setTimeout(() => {
      this.progress = 100;
    }, 8000);
    setTimeout(() => {
      this.$router.push("main");
    }, 10000);
  },
};
</script>

<style lang="scss" scoped>
.OutsideBar {
  height: 35px;
  width: 90%;
  background: #c4c4c4;
  margin: auto;
  border-radius: 50px;
}
.InsideBox {
  height: 35px;
  width: 0%;
  background: #00bfab;
  border-radius: 50px;
}
.TitleBox {
  width: 1024px;
  margin: auto;
}
.fontTitle {
  font-size: 60px;
}
.fontSub {
  font-size: 31px;
}
</style>
