<template>
  <section>
    <div>
      <div class="calculator">
        <div class="card-history-keys">
          <div class="Content">
            <div class="row" v-for="(obj, i) in history" :key="i">
              <div class="col-md-4 history-label-hearder">
                {{ obj.typeName }}
              </div>
              <div class="col-md-8 history-label-hearder">
                {{ obj.date }} - {{ obj.time }}
              </div>
              <div
                class="col-md-12 "
                style="text-align:left;font-size: 25px;padding-left: 20px;"
              >
                {{ obj.sum }}
              </div>
              <div class="col-md-12 history-line-card">
                {{ obj.number }}
              </div>
            </div>
          </div>
          <footer class="Footer">
            <button
              type="button"
              class="equal-sign-bottotm  equal-sign"
              @click="calcullateClear()"
            >
              Clear
            </button>
          </footer>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "FilterHistory",
  props: ["history", "setdata"],
  data() {
    return {
      historyList: []
    };
  },
  methods: {
    calcullateClear() {
      localStorage.removeItem("calculate");
      this.$emit("setdata", []);
    }
  }
};
</script>

<style scoped>
.calculator {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 550px;
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
  width: 350px;
  padding-top: 30px;
}
.calculator-screen2 {
  width: 100%;
  font-size: 3rem;
  height: 60px;
  border: none;
  background-color: #fff;
  text-align: left;
  padding-right: 20px;
  padding-left: 10px;
  width: 350px;
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

.equal-sign {
  background-color: #f17377;
  border-color: #f17377;
  color: rgb(109, 105, 105);
  height: 100%;
  grid-area: 5 / 4 / 5 / 6;
}
.equal-sign-bottotm {
  height: 50px;
  background-color: #f17377 !important;
  border-radius: 15px;
  border: 0px;
  color: white;
  font-size: 30px;
  width: 100px;
}
.spa .equal-sign:hover {
  background-color: rgb(109, 105, 105);
}

.card-history-keys {
  grid-gap: 20px;
  padding: 20px;
  height: 565px;
}

.Content {
  height: 100%;
  overflow: auto;
  padding-top: 10px;
  padding-bottom: 40px;
}
.Footer {
  float: right;
  position: relative;
  margin-top: -50px;
  clear: both;
}
::-webkit-scrollbar {
  width: 0px;
}
.history-line-card {
  text-align: left;
  border-top-style: solid;
  border-top-color: #a3a3a3;
  border-top-width: 3px;
  font-size: 20px;
  padding-left: 20px;
}
.history-label-hearder {
  font-size: 25px;
  float: right;
  font-weight: 500;
}
@media screen and (max-width: 768px) {
  .calculator {
    width: auto;
  }
}
</style>
