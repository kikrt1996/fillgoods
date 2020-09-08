<template>
  <div class="content">
    <div class="row">
      <div class="col column">
        <div class="labelHeaderDetail"><label>Calculator A</label></div>
        <CalculateDetailA @setdata="setDataHistory" />
      </div>
      <div class="col column">
        <div class="labelHeaderDetail"><label>Calculator B</label></div>
        <CalculateDetailB @setdata="setDataHistory" />
      </div>
      <div class="col column">
        <div class="labelHeaderDetail">
          <label>Results</label>
          <input
            class="input-fillter"
            type="text"
            v-model="dataObj.filter"
            placeholder="Search by result,date"
            @input="fillterHistory()"
          />
          <select
            id="ddlViewBy"
            class="dropDown-fillter"
            v-model="dataObj.filterType"
            @change="fillterHistory()"
          >
            <option value="All" selected="selected">ALL</option>
            <option value="A">Calculator A</option>
            <option value="B">Calculator B</option>
          </select>
        </div>
        <FilterHistory @setdata="setDataHistory" :history="tempHistoryList" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CalculateDetailA from "./CalculateDetailA";
import CalculateDetailB from "./CalculateDetailB";
import FilterHistory from "./FilterHistory";
export default {
  name: "CalculateDetail",
  components: {
    CalculateDetailA,
    CalculateDetailB,
    FilterHistory
  },
  data() {
    return {
      labelForShow: "",
      sumTotal: "",
      isDot: false,
      ismark: true,
      checkForDot: [],
      numMark: [],
      totalCalCulate: 0,
      historyList: [],
      dataObj: {
        filter: "",
        filterType: "All"
      },
      tempHistoryList: []
    };
  },
  mounted() {
    let arr = (this.historyList = JSON.parse(
      localStorage.getItem("calculate")
    ));
    this.historyList = arr ? arr : [];
    this.fillterHistory();
  },
  methods: {
    setDataHistory(event, value) {
      this.historyList = event;
      this.fillterHistory();
    },
    async fillterHistory() {
      let filter = this.dataObj.filter ? this.dataObj.filter.toLowerCase() : "";
      let arr = [];
      if (this.dataObj.filterType !== "All") {
        arr = await this.historyList.filter(e =>
          e.type.includes(this.dataObj.filterType)
        );
        this.filterType(arr, filter);
      } else {
        arr = this.historyList.filter(
          e =>
            e.typeName.toLowerCase().includes(filter) ||
            e.date.toLowerCase().includes(filter) ||
            e.time.toLowerCase().includes(filter) ||
            e.sum.includes(filter) ||
            e.number.toLowerCase().includes(filter)
        );
        this.tempHistoryList = arr;
      }
    },
    filterType(arr, filter) {
      let newArr = arr.filter(
        e =>
          e.typeName.toLowerCase().includes(filter) ||
          e.date.toLowerCase().includes(filter) ||
          e.time.toLowerCase().includes(filter) ||
          e.sum.includes(filter) ||
          e.number.toLowerCase().includes(filter)
      );
      this.tempHistoryList = newArr;
    }
  }
};
</script>

<style scoped>
.label-for-bottom {
  padding-top: 10px;
}

html,
body {
  margin: 0px;
  height: 100%;
}

.box {
  background: red;
  position: absolute;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
}

.content {
  padding: 10px;
  height: auto;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1);
  transition: 0.3s;
  width: auto;
  border-radius: 30px;
}
.content:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

.column {
  float: left;
  width: auto;
  padding: 15px;
  height: auto;
}
.labelHeaderDetail {
  text-align: left;
  font-size: 20px;
}
.dropDown-fillter {
  background-color: white;
  color: black;
  border-radius: 15px;
  width: 140px;
  border: 1px solid grey;
  outline: none;
  height: 35px;
}
.input-fillter {
  background-color: white;
  color: black;
  border-radius: 15px;
  border: 1px solid grey;
  outline: none;
  padding-left: 5px;
  /* width: 140px; */
}
</style>
