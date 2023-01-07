<template>
  <div>
    <select v-model="currentCountry" ref="city">
      <option v-for="ulke in ulkeler" :key="ulke" :value="ulke">
        {{ ulke }}
      </option>
    </select>
    <button @click="gonder()">Search</button>
    <br />
    <div v-if="isVisible" class="arele lead">
      <div class="row">
        <div class="col-5"><img :src="flag" :alt="name" /></div>
        <div class="col-7">
          <h2>{{ currentCountry }}</h2>
          <hr />
          <section>
            <div class="row">
              <div class="col-3"><b>Population</b></div>
              <div class="col-9">{{ population }}</div>
            </div>
            <div class="row">
              <div class="col-3"><b>Capital</b></div>
              <div class="col-9">{{ capital }}</div>
            </div>
            <div class="row">
              <div class="col-3"><b>Language</b></div>
              <div class="col-9">{{ language }}</div>
            </div>
            <div class="row">
              <div class="col-3"><b>Continent</b></div>
              <div class="col-9">{{ continent }}</div>
            </div>
            <div class="row">
              <div class="col-3"><b>Currencies</b></div>
              <div class="col-9">
                {{ currencies }} ( {{ currenciesSymbol }} )
              </div>
            </div>
          </section>
        </div>
      </div>
      <div class="roww">
        <ul>
          <li v-for="(border, index) in borders" :key="border">
            {{ komsuUlkeName[index] }} <br />

            <img
              :src="this.bordersFlags[index]"
              @click="yenidenGoster(index)"
            />
          </li>
        </ul>
      </div>
      <p id="neighborCountry-details">{{ pinnerText }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      isVisible: false,
      ulkeUrl: "https://api.covid19api.com/countries",
      url: "https://restcountries.com/v3.1/name/",
      komsuUrl: "https://restcountries.com/v3.1/alpha?codes=",
      currentCountry: "",
      capital: "",
      population: "",
      language: "",
      continent: "",
      currencies: "",
      currenciesSymbol: "",
      datas: [],
      flag: "",
      ulkeler: [],
      name: "",
      borders: [],
      bordersFlags: [],
      komsuUlkeName: [],
      pinnerText: "",
    };
  },
  created() {
    axios.get(this.ulkeUrl).then((response) => {
      for (let i = 0; i < 240; i++) {
        this.ulkeler[i] = response.data[i].Country;
      }
    });
  },

  methods: {
    gonder() {
      (this.bordersFlags = []),
        (this.komsuUlkeName = []),
        (this.isVisible = true),
        (this.pinnerText = "");
      axios.get(this.url + this.currentCountry).then((response) => {
        // console.log(response.data[0]);
        const data = response.data[0];
        this.name = data.name.common;
        this.capital = data.capital.toString();
        this.flag = data.flags.png;
        this.population = data.population;
        this.language = Object.values(data.languages)[0];
        this.continent = data.continents[0];
        this.currencies = Object.values(data.currencies)[0].name;
        this.currenciesSymbol = Object.values(data.currencies)[0].symbol;
        this.borders = data.borders;

        try {
          this.borders.length;
        } catch (error) {
          this.pinnerText = this.currentCountry + " doesn't have borderer.";
        }
        for (let i = 0; i < this.borders.length; i++) {
          const ulkeCode = this.borders[i];
          axios.get(this.komsuUrl + ulkeCode).then((response) => {
            const data = response.data[0];
            this.bordersFlags.push(data.flags.png);
            this.komsuUlkeName.push(data.name.common);
          });
        }
      });
    },
    yenidenGoster(index) {
      const degerr = this.komsuUlkeName[index];
      // console.log(degerr);
      this.currentCountry = degerr;
      this.gonder();
    },
  },
  filters: {
    toUpperCase(value) {
      return value.toUpperCase();
    },
    toLowerCase(value) {
      return value.toLowerCase();
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
select {
  margin-top: 20px;
  width: 50%;
  padding: 10px;
  border-radius: 10px;
}
img {
  width: 220px;
  height: 150px;
}
button {
  padding: 10px;
  border-radius: 10px;
  border: none;
}
.arele {
  width: 60vw;
  padding: 30px 30px 30px 30px;
  margin: 25px auto;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 20px;
  background-color: #3d5656c4;
}
ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
}
section {
  margin-top: 14px;
}
.col-7 {
  text-align: left;
  font-size: 16px;
}
.col-3 {
  text-align: right;
  padding-right: 10px;
  font-size: 16px;
  font-weight: 800;
}
.roww {
  margin-top: 40px;
}
h2 {
  margin-bottom: 14px;
  font-weight: 700;
}

.col-5 {
  display: flex;
  justify-content: center;
  align-items: center;
}
.roww img {
  width: 120px;
  height: 80px;
  margin: 0 auto;
}

@media (max-width: 1200px) {
  .col-7 {
    width: 100vw;
  }
  .col-5 {
    width: 100%;
  }
  .col-9 {
    width: 100vw;
    text-align: center;
  }
  .col-3 {
    width: 100%;
    text-align: center;
  }
  h2 {
    text-align: center;
  }
  .arele {
    width: 90vw;
  }
}
</style>
