<template>
  <div style="width: 100%; height: 100vh; display: flex; align-items: center; justify-content: center">
    <div style="width: 200px">
      <v-text-field @keyup.enter="createData()" v-model="value" outlined class="mb-1" hide-details placeholder="somesing..." />
      <v-btn @click="createData()" block depressed height="55px" color="success">send</v-btn>
      <h1 class="mt-5">{{ data }}</h1>
    </div>
  </div>
</template>

<script>
import { set, get } from "idb-keyval";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      value: "",
      data: "",
    };
  },
  async created() {
    await this.getData();
  },
  methods: {
    async createData() {
      if (this.value) {
        await set("token", this.value);
        this.getData();
      } else {
        alert("ma'lumot kiritilmadi");
      }
    },
    async getData() {
      this.data = await get("token");
      this.value = "";
    },
  },
};
</script>
