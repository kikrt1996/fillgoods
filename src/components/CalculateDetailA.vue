<template>
  <section>
    <div>
      <div class="calculator">
        <input
          type="text"
          class="calculator-screen"
          v-model="totalCalCulate"
          disabled
        />
        <input
          type="text"
          class="calculator-screen2"
          v-model="labelForShow"
          disabled
        />

        <div class="calculator-keys">
          <button
            v-for="(num, i) in number"
            :key="i"
            type="button"
            @click="setNumberForcal(num, i)"
          >
            <label>{{ num.label }}</label>
          </button>
          <button
            type="button"
            class="spanCollum0"
            @click="setNumberForcal({ value: '0', label: '0' })"
          >
            0
          </button>
          <button
            type="button"
            class="decimal"
            @click="setNumberForcal({ value: '.', label: '.' })"
          >
            .
          </button>

          <button
            type="button"
            class="equal-divide"
            @click="setNumberForcal({ value: '/', label: '/' })"
          >
            &divide;
          </button>
          <button
            type="button"
            class="equal-sign"
            @click="calculate('setLocal')"
          >
            =
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "CalculateDetailA",
  props: ["setdata"],
  data() {
    return {
      number: [
        { value: "c", label: "C" },
        { value: "+", label: "+" },
        { value: "-", label: "-" },
        { value: "*", label: "x" },
        { value: "7", label: "7" },
        { value: "8", label: "8" },
        { value: "9", label: "9" },
        { value: "4", label: "4" },
        { value: "5", label: "5" },
        { value: "6", label: "6" },
        { value: "1", label: "1" },
        { value: "2", label: "2" },
        { value: "3", label: "3" }
      ],
      labelForShow: "",
      sumTotal: "",
      isDot: false,
      ismark: true,
      checkForDot: [],
      numMark: [],
      totalCalCulate: 0,
      tempTotalCalCulate: 0,
      tempDataForLocal: []
    };
  },
  methods: {
    setNumberForcal(el, i) {
      if (el.value.toLowerCase() == "c") {
        this.labelForShow = "";
        this.sumTotal = "";
        this.isDot = false;
        this.ismark = true;
        this.checkForDot = [];
        this.numMark = [];
        this.totalCalCulate = 0;
        this.tempTotalCalCulate = 0;
      } else {
        this.setDataForShow(el);
      }
    },
    setDataForShow(el) {
      switch (isNaN(el.value)) {
        case true:
          if (this.labelForShow) {
            if (this.labelForShow.slice(-1) == ".") {
              this.labelForShow += "00";
              this.sumTotal += "00";
            }
          }
          if (el.value == "." && !this.isDot) {
            this.setDot(el);
          } else if (el.value != "." && this.ismark) {
            if (!this.labelForShow && el.value == "-") {
              this.labelForShow += el.label.toLowerCase() + " ";
              this.sumTotal += el.value;
              this.ismark = false;
            } else if (this.labelForShow) {
              this.numMark.push(el.value);
              this.checkDot(el);
              this.setNumberForCale(el);
            }
          }
          break;
        default:
          this.labelForShow += el.label;
          this.sumTotal += el.value;
          this.ismark = true;
          if (this.checkForDot.length >= 1) {
            this.checkForDot.push(el);
          }
          break;
      }
    },
    async setNumberForCale(el) {
      if (this.numMark.length >= 2) {
        this.checkForDot = [];
        await this.calculate();
        this.checkForDot.push(this.tempTotalCalCulate, el.label);
      }
      this.labelForShow += " " + el.label.toLowerCase() + " ";
      this.sumTotal += el.value;
      this.isDot = false;
      this.ismark = false;
    },
    checkDot(el) {
      let temp =
        this.checkForDot.length > 0
          ? this.checkForDot
          : this.labelForShow.split(el.value);
      temp.push(el.value);
      this.checkForDot = temp;
    },
    setDot(el) {
      if (this.checkForDot.length > 1) {
        if (!this.checkForDot[2]) {
          this.labelForShow += +"0" + el.label.toLowerCase();
          this.sumTotal += +"0" + el.value;
          this.isDot = true;
          this.ismark = true;
          this.checkForDot = [];
        } else {
          this.labelForShow += el.label.toLowerCase();
          this.sumTotal += el.value;
          this.isDot = true;
          this.ismark = true;
        }
      } else {
        if (!this.labelForShow && el.label == ".") {
          this.labelForShow += "0" + el.label.toLowerCase();
          this.sumTotal += "0" + el.label.toLowerCase();
        } else {
          this.labelForShow += el.label.toLowerCase();
          this.sumTotal += el.value;
        }

        this.isDot = true;
        this.ismark = true;
      }
    },
    calculate(actions) {
      const res = encodeURIComponent(this.sumTotal);
      if (res) {
        axios.get(`http://api.mathjs.org/v4/?expr=${res}`).then(res => {
          this.tempTotalCalCulate = res.data;
          if (actions == "setLocal") {
            this.totalCalCulate = res.data;
            this.setdataForLocal();
          }
        });
      }
    },
    setdataForLocal() {
      this.tempDataForLocal = JSON.parse(localStorage.getItem("calculate"));
      const calculateA = this.tempDataForLocal ? this.tempDataForLocal : [];
      calculateA.push({
        date: moment().format("DD/MM/YYYY"),
        time: moment().format("HH:mm:ss"),
        number: this.labelForShow,
        sum: this.totalCalCulate.toString(),
        type: "A",
        typeName: "Calculator A"
      });
      this.tempDataForLocal = calculateA;
      localStorage.setItem("calculate", JSON.stringify(calculateA));
      this.$emit("setdata", this.tempDataForLocal);
    }
  }
};
</script>

<style scoped>
.calculator {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 350px;
  border-radius: 30px;
}

.calculator-screen {
  width: 100%;
  font-size: 3rem;
  height: 90px;
  border: none;
  background-color: #fff;
  text-align: left;
  padding-right: 20px;
  padding-left: 10px;
  border-radius: 30px;
  width: 300px;
  padding-top: 30px;
}
.calculator-screen2 {
  width: 100%;
  font-size: 2rem;
  height: 60px;
  border: none;
  background-color: #fff;
  text-align: left;
  padding-right: 20px;
  padding-left: 10px;
  width: 300px;
  border-top-style: groove;
}
button {
  height: 60px;
  background-color: #a7d4fa !important;
  border-radius: 15px;
  border: 0px;
  background-color: transparent;
  font-size: 2rem;
  color: rgb(109, 105, 105);
  background-image: linear-gradient(
    to bottom,
    transparent,
    transparent 50%,
    rgba(0, 0, 0, 0.04)
  );
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.05),
    inset 0 1px 0 0 rgba(255, 255, 255, 0.45),
    inset 0 -1px 0 0 rgba(255, 255, 255, 0.15),
    0 1px 0 0 rgba(255, 255, 255, 0.15);
  text-shadow: 0 1px rgba(255, 255, 255, 0.4);
}
button:focus {
  /* outline: 1px dotted; */
  outline: none;
  color: #b0353a;
}
button:hover {
  background-color: #eaeaea;
}

.operator {
  color: rgb(109, 105, 105);
}

.all-clear {
  background-color: #f0595f;
  border-color: #b0353a;
  color: #fff;
}

.all-clear:hover {
  background-color: #f17377;
}

.equal-sign {
  background-color: #a7d4fa;
  border-color: #a7d4fa;
  color: rgb(109, 105, 105);
  height: 100%;
  grid-area: 4 / 4 / 6 / 5;
}
.equal-divide {
  background-color: #a7d4fa;
  border-color: #a7d4fa;
  color: rgb(109, 105, 105);
  height: 100%;
  grid-area: 2 / 4 / 4 / 4;
}
.spanCollum0 {
  grid-area: 5/ 1 / span 1 / span 2;
}

.equal-sign:hover {
  background-color: rgb(109, 105, 105);
}

.calculator-keys {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 20px;
  padding: 20px;
}

@media screen and (max-width: 768px) {
  .calculator {
    width: auto;
  }
}
</style>
