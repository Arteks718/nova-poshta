<template>
  <div>
    <p>Населений пункт</p>
    <select v-model="city" @change="getWarehouses">
      <option v-for="(city, index) in cities">
        {{ city }}
      </option>
    </select>
    <p>Поштове відділення</p>
    <select>
      <option v-for="warehouse in warehouses">
        {{ warehouse }}
      </option>
    </select>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      cities: [],
      city: "",
      warehouses: [],
      apiKey: "8c6d6da608aeb0f1810f49ed433e0d58",
    };
  },
  mounted() {
    axios
      .post("https://api.novaposhta.ua/v2.0/json/", {
        apiKey: this.apiKey,
        modelName: "Address",
        calledMethod: "getCities",
        methodProperties: {},
      })
      .then((response) => {
        response.data.data.forEach((element) => {
          this.cities.push(element.Description);
        });
      });
  },
  methods: {
    getWarehouses() {
      axios
        .post("https://api.novaposhta.ua/v2.0/json/", {
          apiKey: this.apiKey,
          modelName: "Address",
          calledMethod: "getWarehouses",
          methodProperties: {
            CityName: this.city,
          },
        })
        .then((response) => {
          this.warehouses = [];
          response.data.data.forEach((element) => {
            this.warehouses.push(element.Description);
          });
        });
    },
  },
};
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
  select {
    width: 300px;
  }
}
</style>
