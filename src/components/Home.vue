<template>
  <div class="home">
    <div class="select__wrap">
      <span>Населений пункт</span>
      <select @change="getWarehouses" v-model="currentCity">
        <option :value="null">Выберете область</option>
        <option
          v-for="(area, index) in allAreas"
          :key="index"
          :value="area.AreasCenter"
        >
          {{ area.Description }}
        </option>
      </select>
    </div>

    <div class="select__wrap" v-if="warehouses.length > 0">
      <span>Поштове відділення</span>
      <select>
        <option
          v-for="(wh, index) in warehouses"
          :key="index"
          :value="wh.SettlementRef"
        >
          {{ wh.Description }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const key = "fd92916270aeaf36145d47e72f109342";

export default {
  name: "Home",
  props: {},
  data: function () {
    return {
      key: key,
      allAreas: [],
      cities: [],
      warehouses: [],
      currentCity: null,
    };
  },
  mounted: async function () {
    const { data } = await axios.post("https://api.novaposhta.ua/v2.0/json/", {
      apiKey: "fd92916270aeaf36145d47e72f109342",
      modelName: "Address",
      calledMethod: "getAreas",
      methodProperties: {},
    });

    this.allAreas = data.data;
  },
  methods: {
    getWarehouses: async function () {
      const { data } = await axios.post(
        "https://api.novaposhta.ua/v2.0/json/",
        {
          apiKey: this.key,
          modelName: "AddressGeneral",
          calledMethod: "getWarehouses",
          methodProperties: {
            CityRef: this.currentCity,
            FindByString: "відділення",
          },
        }
      );

      this.warehouses = data.data;
    },
  },
};
</script>

<style scoped>
select {
  font-size: 16px;
  padding: 10px 10px;
  margin-bottom: 20px;
  max-width: 500px;
  border-radius: 4px;
  outline: none;
}

.select__wrap {
  display: flex;
  flex-direction: column;
}

.select__wrap span {
  font-weight: 700;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  margin-bottom: 10px;
}
</style>
