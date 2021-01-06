<template>
  <div>
    <myheader></myheader>
    <p v-if="msg.length > 0">
      {{ msg }}
    </p>
    <p v-else>no text</p>
    <input type="text" v-model="msg" />
    <button @click="clear()">clear</button>
    <button @click="created()">created</button>
    <br />
    <button @click="microcms()">microcms</button>
    <template v-if="contents != null">
      <h1>{{ contents.title }}</h1>
      <h2>日付: {{ contents.publishedAt }}</h2>
      <p v-html="contents.body"></p>
    </template>
  </div>
</template>

<script>
import myheader from "./components/myheader";
import axios from "axios";
import config from "./config/development";

export default {
  components: {
    myheader,
  },
  data() {
    return {
      msg: "Hello World!",
      contents: null,
    };
  },
  methods: {
    clear() {
      this.msg = "";
    },

    created() {
      console.log("created");
      fetch(
        "https://www.geonames.org/postalCodeLookupJSON?postalcode=10504&country=US"
      )
        .then((response) => {
          console.log(response.status);
          return response.json();
        })
        .then((json) => {
          this.msg = json.postalcodes[0].adminName1;
        })
        .catch((err) => {
          this.msg = err; // エラー処理
        });
    },

    async microcms() {
      try {
        const res = await axios.get(
          "https://iwagami.microcms.io/api/v1/news/xvjaw8sfg",
          {
            headers: {
              "X-API-KEY": config.api_key,
            },
          }
        );
        this.contents = res.data;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>