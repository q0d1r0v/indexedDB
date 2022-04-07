<template>
  <div
    style="
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
    "
  >
    <div style="display: flex; align-items: center; justify-content: center">
      <v-btn class="mx-3" color="success" depressed @click="showDialog('add')"
        >add or update</v-btn
      >
      <v-btn class="mx-3" color="primary" depressed @click="showDialog(get)"
        >get</v-btn
      >
      <v-btn @click="showDeleteDialog()" class="mx-3" color="error" depressed
        >delete</v-btn
      >
    </div>
    <v-dialog v-model="dialog" width="300px">
      <v-card
        width="300px"
        style="
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
        "
        class="pa-5"
      >
        <div style="width: 200px">
          <v-text-field
            v-model="key"
            outlined
            class="mb-1"
            hide-details
            placeholder="key"
          />
          <v-text-field
            v-if="!get"
            v-model="value"
            outlined
            class="mb-1"
            hide-details
            placeholder="value"
          />
          <v-btn
            v-if="!get"
            @click="createData()"
            block
            depressed
            height="55px"
            color="success"
            >send</v-btn
          >
          <v-btn
            v-else
            @click="getData(key)"
            block
            depressed
            height="55px"
            color="success"
            >get</v-btn
          >
        </div>
      </v-card>
    </v-dialog>

    <v-dialog v-model="delete_dialog" width="300px">
      <v-card
        width="300px"
        style="
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
        "
        class="pa-5"
      >
        <div style="width: 200px">
          <v-text-field
            v-model="key"
            outlined
            class="mb-1"
            hide-details
            placeholder="key"
          />
          <v-btn
            @click="deleteItem(key)"
            block
            depressed
            height="55px"
            color="success"
            >delete</v-btn
          >
        </div>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import { set, get, del } from "idb-keyval";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      delete_dialog: false,
      delete: "",
      get: false,
      dialog: false,
      key: "",
      value: "",
      data: "",
    };
  },
  async created() {
    await this.getData();
  },
  methods: {
    showDeleteDialog() {
      this.key = "";
      this.delete_dialog = true;
    },
    deleteItem(key) {
      if (key) {
        del(key);
        this.key = "";
        alert("ma'lumot muvoffaqaiyatli o'chirildi");
      } else {
        alert("ma'lumotni kiriting!");
      }
    },
    showDialog(data) {
      if (data == "add") {
        this.get = false;
        this.key = "";
        this.value = "";
      } else {
        this.get = true;
        this.key = "";
      }
      this.dialog = true;
    },
    async createData() {
      if (this.value && this.key) {
        await set(this.key, this.value);
        this.key = "";
        this.value = "";
        alert("ma'lumot muvoffaqaiyatli yakunlandi");
      } else {
        alert("ma'lumot kiritilmadi");
      }
    },
    async getData(txt) {
      this.data = await get(txt);
      this.key = "";
      console.log(this.data);
      alert("ma'lumot muvoffaqaiyatli yuklab olindi!");
    },
  },
};
</script>
