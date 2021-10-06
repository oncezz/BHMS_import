<template>
  <div class="fullscreen bgimg">
    <!-- header file -->
    <div class="row q-mt-md justify-between">
      <div class="row bgBlack q-pa-sm">
        <div class="q-pr-md">
          <img src="../../public/image/logo.svg" alt="" style="width: 80px" />
        </div>
        <div>
          <div class="fontTitle">BHMS Analysis program</div>
          <div class="fontSub">Burapha Withi Express Health Monitor System</div>
        </div>
      </div>
      <div class="q-pr-md q-pt-sm" v-show="!turnOn">
        <img
          src="../../public/image/turnOff.svg"
          alt=""
          width="75px"
          @click="turnOnBtn"
          class="cursor-pointer"
        />
      </div>
      <div class="q-pr-md q-pt-sm" v-show="turnOn">
        <img
          src="../../public/image/turnOn.svg"
          alt=""
          width="75px"
          @click="turnOnBtn"
          class="cursor-pointer"
        />
      </div>
    </div>
    <!-- หน้าต่างหลักตอนปิด -->
    <div v-show="!turnOn" class="absolute-center">
      <div class="mainDiv">
        <div class="centerW" align="center">Please turn on the system</div>
      </div>
      <div style="height: 20px"></div>
      <div class="row btD q-pa-sm" style="width: 1040px; margin: auto">
        <div class="btDiv q-pa-md">
          <div class="btWU">15 min interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat">-</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">Day time interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat">-</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">Night time interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat">-</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">24 hour interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat">-</div>
        </div>
      </div>
    </div>

    <!-- หน้าต่างใหญ่ตอนเปิด -->
    <div v-show="turnOn" class="absolute-center">
      <div class="mainDiv">
        <div class="row" style="background-color: black">
          <div class="q-pa-md">
            <div class="fRow" align="center">
              <div class="q-pt-md">
                <q-circular-progress
                  show-value
                  font-size="18px"
                  :value="value"
                  size="180px"
                  :thickness="0.3"
                  color="green-8"
                  track-color="grey-3"
                  class="q-ma-md"
                >
                  {{ value }}%
                </q-circular-progress>
              </div>
              <div style="font-size: 16px">5 min</div>
            </div>
          </div>
          <div class="q-pt-md">
            <div class="row">
              <div
                class="col cursor-pointer"
                @click="openDetail(index - 1)"
                v-for="index in 8"
                :key="index"
              >
                <box-display
                  :label="labelCol[index - 1]"
                  :sensorData="sensorData[index - 1]"
                ></box-display>
              </div>
            </div>
            <div class="row">
              <div
                class="col cursor-pointer"
                @click="openDetail(index + 7)"
                v-for="index in 8"
                :key="index"
              >
                <box-display
                  :label="labelCol[index + 7]"
                  :sensorData="sensorData[index + 7]"
                ></box-display>
              </div>
            </div>
            <div class="row">
              <div
                class="cursor-pointer"
                @click="openDetail(index + 15)"
                v-for="index in 3"
                :key="index"
              >
                <box-display
                  :label="labelCol[index + 15]"
                  :sensorData="sensorData[index + 15]"
                ></box-display>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div style="height: 20px"></div>
      <div class="row btD q-pa-sm" style="width: 1040px; margin: auto">
        <div class="btDiv q-pa-md">
          <div class="btWU">15 min interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat" v-show="turnOn">{{ label15min }}</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">Day time interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat" v-show="turnOn">{{ labelDayTime }}</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">Night time interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat" v-show="turnOn">{{ labelNightTime }}</div>
        </div>
        <div class="btDiv q-pa-md">
          <div class="btWU">24 hour interval</div>
          <div class="btWT">The lastest updated time</div>
          <div class="btDat" v-show="turnOn">{{ label24hour }}</div>
        </div>
      </div>
    </div>

    <!-- ใส่พาสเวิด -->
    <q-dialog v-model="dialogInput" persistent>
      <q-card>
        <div class="diaBox">
          <div class="inDiabox">
            <div class="q-pa-sm" align="right">
              <q-icon
                class="cursor-pointer"
                @click="closeDia"
                name="fas fa-times"
                size="20px"
              ></q-icon>
            </div>
            <div class="q-pt-sm" style="font-size: 26px" align="center">
              Please input password
            </div>
            <div class="passwordBox">
              <q-input
                type="password"
                v-model="password"
                placeholder="Please input password here"
                dark
                ref="password"
                @keyup.enter="turnOnLoginBtn"
              />
            </div>

            <div class="q-pt-lg" align="center">
              <q-btn
                @click="turnOnLoginBtn"
                v-show="!turnOn"
                color="positive"
                label="Turn on"
                no-caps
              />
              <q-btn
                @click="turnOnLoginBtn"
                v-show="turnOn"
                color="negative"
                label="Turn off"
                no-caps
              />
            </div>
          </div>
        </div>
      </q-card>
    </q-dialog>

    <!-- ใส่ค่าในเซ็นเซอร์ -->
    <q-dialog v-model="dialogDetail" persistent>
      <q-card>
        <div class="outDetailDia">
          <div class="inDetailDia">
            <div class="q-pa-xs" align="right">
              <q-icon
                class="cursor-pointer"
                @click="closeDetail"
                name="fas fa-times"
                size="30px"
              ></q-icon>
            </div>

            <div class="row q-pl-md">
              <div style="width: 50%">
                <div style="font-size: 50px">{{ detailName }}</div>
                <div style="font-size: 16px">Initial strain adjustment</div>
              </div>
              <div class="col q-pt-xl" align="center">
                <div>sensor 5</div>
                <div class="sensorBox row">
                  <div class="q-px-md" style="width: 80px">
                    <q-input v-model="strain5" dark dense />
                  </div>
                  <div class="q-pt-md">µε</div>
                </div>
              </div>
            </div>
            <div class="row q-pt-lg">
              <div class="outBox" align="center">
                <div>sensor 1</div>
                <div class="sensorBox row">
                  <div class="q-px-md" style="width: 80px">
                    <q-input v-model="strain1" dark dense />
                  </div>
                  <div class="q-pt-md">µε</div>
                </div>
              </div>
              <div class="outBox" align="center">
                <div>sensor 2</div>
                <div class="sensorBox row">
                  <div class="q-px-md" style="width: 80px">
                    <q-input v-model="strain2" dark dense />
                  </div>
                  <div class="q-pt-md">µε</div>
                </div>
              </div>
              <div class="outBox" align="center">
                <div>sensor 3</div>
                <div class="sensorBox row">
                  <div class="q-px-md" style="width: 80px">
                    <q-input v-model="strain3" dark dense />
                  </div>
                  <div class="q-pt-md">µε</div>
                </div>
              </div>
              <div class="outBox" align="center">
                <div>sensor 4</div>
                <div class="sensorBox row">
                  <div class="q-px-md" style="width: 80px">
                    <q-input v-model="strain4" dark dense />
                  </div>
                  <div class="q-pt-md">µε</div>
                </div>
              </div>
            </div>

            <div class="q-pt-lg" align="center">
              <q-btn
                @click="saveIntBtn()"
                style="width: 150px"
                color="positive"
                label="Save"
                no-caps
              />
            </div>
          </div>
        </div>
      </q-card>
    </q-dialog>

    <!-- แบล็คกราวดำ -->
    <div class="bgDrop fullscreen" v-show="dialogInput || dialogDetail"></div>
  </div>
</template>

<script>
import axios from "axios";
import boxDisplay from "../components/boxDisplay.vue";
export default {
  components: {
    boxDisplay,
  },
  data() {
    return {
      speed: 1, //ตัวจำลองความเร็วในการทำงาน
      passwordSetup: "", //รหัสเปิดปิดโปรแกรม
      turnOn: false, //ตัวเปิดปิดโปรแกรม
      value: 0, // ค่าดาวโหลด
      timeCheck: "",
      label15min: "-", // ค่าแสดงใน 15 min
      labelDayTime: "-", // ค่าแสดงใน Day Time
      labelNightTime: "-", // ค่าแสดงใน Night Time
      label24hour: "-", // ค่าแสดงของ 24 hour
      sensorData: [
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
      ], // ตัวเซนเซอร์ 0 สีแดง 1 สีเขียว
      labelCol: [
        "M29/24",
        "M30/01",
        "M30/07",
        "M31/02",
        "M32/11",
        "M33/02",
        "M34/02",
        "M34/05",
        "M35/23",
        "M36/16",
        "M36/18",
        "M36/20",
        "M38/17",
        "M39/06",
        "M40/40",
        "M41/09",
        "M42/04",
        "M43/03",
        "M43/19",
      ],
      strain1: 0,
      strain2: 0,
      strain3: 0,
      strain4: 0,
      strain5: 0,
      detailName: "", // ชื่อเซนเซอร์ที่แสดงในกล่องดีเทล
      dialogInput: false, //  เปิดหน้าต่างใส่พาสเวิด
      password: "", //      เก็บพาสเวิด
      dialogDetail: false, // ตั้งค่าเซ็นเซอ
      serverPath: "http://localhost/bhms_data/",
      colId: 0,
    };
  },
  methods: {
    async openDetail(index) {
      this.colId = index;
      this.detailName = this.labelCol[index];
      let url = this.serverPath + "loadinit.php?col=" + index;
      let res = await axios.get(url);

      this.strain1 = res.data[0];
      this.strain2 = res.data[1];
      this.strain3 = res.data[2];
      this.strain4 = res.data[3];
      this.strain5 = res.data[4];
      this.dialogDetail = true;
    },
    async saveIntBtn() {
      let url =
        this.serverPath +
        "saveint.php?col=" +
        this.colId +
        "&s1=" +
        this.strain1 +
        "&s2=" +
        this.strain2 +
        "&s3=" +
        this.strain3 +
        "&s4=" +
        this.strain4 +
        "&s5=" +
        this.strain5;
      let res = await axios.get(url);
      this.$q.notify({
        message: "data saved",
        color: "positive",
      });
      this.dialogDetail = false;
    },
    turnOnBtn() {
      this.dialogInput = true;
      setTimeout(() => {
        this.$refs.password.$el.focus();
      }, 20);
    },
    closeDia() {
      this.dialogInput = false;
    },
    closeDetail() {
      this.dialogDetail = false;
    },
    async turnOnLoginBtn() {
      if (this.password == this.passwordSetup) {
        this.turnOn = !this.turnOn;
        this.dialogInput = !this.dialogInput;
        this.password = "";
        if (this.turnOn) {
          //ทำการ load ข้อมูลที่ตกค้าง
          let url = this.serverPath + "loadolddata.php";
          let res = await axios.get(url);

          await this.loadCurrentData();
          this.value = 0;
          this.timeCheck = setInterval(async () => {
            this.value += 1;
            if (this.value > 100) {
              this.value = 1;
              let url = this.serverPath + "loadolddata.php";
              let res = await axios.get(url);
              await this.loadCurrentData();
              await this.loadDayTimeData();
              await this.loadNightTimeData();
              await this.load24hourData();
            }
          }, 3000 / this.speed);
        } else {
          clearInterval(this.timeCheck);
        }
      } else {
        this.$q.notify({
          message: "Password incorrect",
          color: "negative",
        });
        this.password = "";
      }
    },
    async loadPassword() {
      let url = this.serverPath + "loadpassword.php";
      let res = await axios.get(url);
      this.passwordSetup = res.data[0].password;
    },
    async loadCurrentData() {
      //ทำการ load ข้อมูลล่าสุด
      let url = this.serverPath + "loadcurrentdata.php";
      let res = await axios.get(url);
      if (res.data != "NR") {
        this.sensorData = [];
        for (let i = 0; i < res.data.length; i++) {
          this.sensorData.push(res.data[i]);
        }
        //update เวลา
        let currentDate = new Date();
        this.label15min =
          currentDate.getDate() +
          "/" +
          (currentDate.getMonth() + 1) +
          "/" +
          currentDate.getFullYear() +
          " " +
          currentDate.getHours() +
          ":" +
          ("0" + currentDate.getMinutes()).slice(-2);
      }
    },
    async loadDayTimeData() {
      //ทำการ load ข้อมูลล่าสุด
      let url = this.serverPath + "loaddaytimedata.php";
      let res = await axios.get(url);
      if (res.data == "updated") {
        let currentDate = new Date();
        this.labelDayTime =
          currentDate.getDate() +
          "/" +
          (currentDate.getMonth() + 1) +
          "/" +
          currentDate.getFullYear() +
          " " +
          currentDate.getHours() +
          ":" +
          ("0" + currentDate.getMinutes()).slice(-2);
      }
    },
    async loadNightTimeData() {
      //ทำการ load ข้อมูลล่าสุด
      let url = this.serverPath + "loadnighttimedata.php";
      let res = await axios.get(url);
      if (res.data == "updated") {
        let currentDate = new Date();
        this.labelNightTime =
          currentDate.getDate() +
          "/" +
          (currentDate.getMonth() + 1) +
          "/" +
          currentDate.getFullYear() +
          " " +
          currentDate.getHours() +
          ":" +
          ("0" + currentDate.getMinutes()).slice(-2);
      }
    },
    async load24hourData() {
      //ทำการ load ข้อมูลล่าสุด
      let url = this.serverPath + "load24hourdata.php";
      let res = await axios.get(url);
      if (res.data == "updated") {
        let currentDate = new Date();
        this.label24hour =
          currentDate.getDate() +
          "/" +
          (currentDate.getMonth() + 1) +
          "/" +
          currentDate.getFullYear() +
          " " +
          currentDate.getHours() +
          ":" +
          ("0" + currentDate.getMinutes()).slice(-2);
      }
    },
  },
  mounted() {
    this.loadPassword();
  },
};
</script>

<style lang="scss" scoped>
.bgBlack {
  background-color: rgba($color: #000000, $alpha: 0.5);
  border-radius: 0px 20px 20px 0px;
}
.fontTitle {
  font-size: 23px;
  padding-top: 10px;
  padding-right: 15px;
}
.fontSub {
  font-size: 12px;
}
.bgimg {
  background-image: url("../../public/image/bg.jpg");
  background-size: cover;
}
.mainDiv {
  background-color: rgba($color: #000000, $alpha: 0.5);
  width: 1040px;
  height: 300px;
  margin: auto;
}
.centerW {
  font-size: 50px;
  line-height: 300px;
}
.btD {
  background-color: rgba($color: #000000, $alpha: 0.5);
  backface-visibility: 50%;
}
.btDiv {
  background-color: rgba($color: #c4c4c4, $alpha: 0.25);
  margin: auto;
  width: 24%;
  height: 130px;
}
.btWU {
  font-size: 20px;
}
.btWT {
  font-size: 12px;
}
.btDat {
  padding-top: 10px;
  text-align: center;
  font-size: 24px;
}
.fRow {
  width: 300px;
  height: 260px;
  background-color: rgba($color: #c4c4c4, $alpha: 0.25);
}
.diaBox {
  width: 400px;
  height: 260px;
  background-color: rgba($color: #000000, $alpha: 0.5);
}
.bgDrop {
  background-color: rgba($color: #202541, $alpha: 0.75);
}
.inDiabox {
  position: relative;
  top: 2%;
  height: 96%;
  width: 96%;
  margin: auto;
  background-color: rgba($color: #333030, $alpha: 1);
}
.passwordBox {
  margin: auto;
  width: 50%;
}
.outDetailDia {
  background-color: rgba($color: #000000, $alpha: 0.5);
  width: 500px;
  height: 340px;
}
.inDetailDia {
  position: relative;
  top: 2%;
  height: 96%;
  width: 96%;
  margin: auto;
  background-color: rgba($color: #333030, $alpha: 1);
}
.outBox {
  width: 25%;
}
.sensorBox {
  margin-top: 5px;
  border-radius: 5px;
  width: 100px;
  height: 45px;
  border: 1px solid white;
} //กล่องตั้งค่าเซ็นเซอร์
</style>
