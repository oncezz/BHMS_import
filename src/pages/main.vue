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
          @click="turnOnBtn()"
          class="cursor-pointer"
        />
      </div>
      <div class="q-pr-md q-pt-sm" v-show="turnOn">
        <img
          src="../../public/image/turnOn.svg"
          alt=""
          width="75px"
          @click="turnOnBtn()"
          class="cursor-pointer"
        />
      </div>
    </div>
    <!-- หน้าต่างหลักตอนปิด -->
    <div style="height: 30px"></div>
    <div class="mainDiv" v-show="!turnOn">
      <div class="centerW" align="center">Please turn on the system</div>
    </div>
    <!-- หน้าต่างใหญ่ตอนเปิด -->
    <div class="mainDiv" v-show="turnOn">
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
            <div style="font-size: 16px">15 min</div>
          </div>
        </div>
        <div class="q-pt-md">
          <div class="row col">
            <box-display
              v-for="index in 8"
              :key="index"
              :label="labelCol[index - 1]"
              :sensorData="sensorData[index - 1]"
            ></box-display>
          </div>
          <div class="row col">
            <box-display
              v-for="index in 8"
              :key="index"
              :label="labelCol[index + 7]"
              :sensorData="sensorData[index + 7]"
            ></box-display>
          </div>
          <div class="row col">
            <box-display
              v-for="index in 3"
              :key="index"
              :label="labelCol[index + 15]"
              :sensorData="sensorData[index + 15]"
            ></box-display>
          </div>
        </div>
      </div>
    </div>
    <!-- 4บล็อคด้านล่าง -->
    <div style="height: 20px"></div>
    <div class="row btD q-pa-sm" style="width: 1024px; margin: auto">
      <div class="btDiv q-pa-md">
        <div class="btWU">15 min interval</div>
        <div class="btWT">The lastest updated time</div>
        <div class="btDat" v-show="!turnOn">-</div>
        <div class="btDat" v-show="turnOn">{{ label15min }}</div>
      </div>
      <div class="btDiv q-pa-md">
        <div class="btWU">Day time interval</div>
        <div class="btWT">The lastest updated time</div>
        <div class="btDat" v-show="!turnOn">-</div>
        <div class="btDat" v-show="turnOn">{{ labelDayTime }}</div>
      </div>
      <div class="btDiv q-pa-md">
        <div class="btWU">Night time interval</div>
        <div class="btWT">The lastest updated time</div>
        <div class="btDat" v-show="!turnOn">-</div>
        <div class="btDat" v-show="turnOn">{{ labelNightTime }}</div>
      </div>
      <div class="btDiv q-pa-md">
        <div class="btWU">24 hour interval</div>
        <div class="btWT">The lastest updated time</div>
        <div class="btDat" v-show="!turnOn">-</div>
        <div class="btDat" v-show="turnOn">{{ label24hour }}</div>
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

    <q-dialog v-model="dialogDetail" persistent>
      <q-card>
        <div class="outDetailDia">
          <div class="inDetailDia">
            <div align="right">
              <q-icon
                class="cursor-pointer"
                @click="closeDetail"
                name="fas fa-times"
                size="30px"
              ></q-icon>
            </div>
            <div style="font-size: 50px">M32/11</div>

            <div class="row">
              <div style="font-size: 16px; width: 50%">
                Initial strain adjustment
              </div>
              <div class="col" align="center">
                <div>sensor 5</div>
                <div class="sensorBox row">
                  <div class="brx" style="width: 80px">
                    <q-input class="brx" v-model="strain" dark dense />
                  </div>
                  <div>µε</div>
                </div>
              </div>
            </div>
            <q-input class="brx" v-model="strain" dark dense />
          </div>
        </div>
      </q-card>
    </q-dialog>
    <!-- แบล็คกราวดำ -->
    <div class="bgDrop fullscreen" v-show="dialogInput"></div>
  </div>
</template>

<script>
import boxDisplay from "../components/boxDisplay.vue";
export default {
  components: {
    boxDisplay,
  },
  data() {
    return {
      turnOn: false, //ตัวเปิดปิดโปรแกรม
      value: 80, // ค่าดาวโหลด
      label15min: "29/08/2021 16:02", // ค่าแสดงใน 15 min
      labelDayTime: "28/08/2021 18:02", // ค่าแสดงใน Day Time
      labelNightTime: "29/08/2021 6:02", // ค่าแสดงใน Night Time
      label24hour: "29/08/2021 0:02", // ค่าแสดงของ 24 hour
      sensorData: [
        [0, 1, 1, 1, 0],
        [1, 1, 0, 1, 0],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 0, 1, 1],
        [1, 1, 0, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 0, 1, 1],
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
      strain: "",
      dialogInput: false, //  เปิดหน้าต่างใส่พาสเวิด
      password: "", //      เก็บพาสเวิด
      dialogDetail: true,
    };
  },
  methods: {
    turnOnBtn() {
      this.dialogInput = true;
    },
    closeDia() {
      this.dialogInput = false;
    },
    closeDetail() {
      this.dialogDetail = false;
    },
    turnOnLoginBtn() {
      if (this.password == "4567") {
        this.turnOn = !this.turnOn;
        this.dialogInput = !this.dialogInput;
        this.password = "";
      } else {
        this.$q.notify({
          message: "Password incorrect",
          color: "negative",
        });
        this.password = "";
      }
    },
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
}
.mainDiv {
  background-color: rgba($color: #000000, $alpha: 0.5);
  width: 1024px;
  height: 300px;
  margin: auto;
}
.centerW {
  font-size: 50px;
  line-height: 400px;
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
  padding-top: 30px;
  text-align: center;
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
  width: 500px;
  height: 400px;
  background-color: rgba($color: #000000, $alpha: 0.5);
}
.inDetailDia {
  position: relative;
  top: 2%;
  height: 96%;
  width: 96%;
  margin: auto;
  background-color: rgba($color: #333030, $alpha: 1);
}
.sensorBox {
  margin-top: 5px;
  width: 100px;
  height: 30px;
  border: 1px solid white;
} //กล่องตั้งค่าเซ็นเซอร์
</style>
