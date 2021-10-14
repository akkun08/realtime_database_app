<template>
  <section class="alert alert-primary">
    <h1>{{ data.title }}</h1>
    <p>{{ data.message }}</p>
    <div class="row my-2">
      <div class="col-sm-auto">
        <input type="text" v-model="data.find" class="form-control" />
      </div>
      <button class="col-sm-auto btn btn-primary" @click="getData">
        Click
      </button>
    </div>
    <div class="alert alert-light">{{ data.fire_data }}</div>
  </section>
</template>

<script>
import axios from "axios";
import { onMounted, reactive } from "vue";

let url =
  "https://akkun-vue3-50410-default-rtdb.firebaseio.com/person.json?orderBy=%22$key%22&equalTo=%22";

export default {
  setup(props) {
    const data = reactive({
      title: "Firebase",
      message: "This is Firebase sample.",
      find: "",
      fire_data: {},
    });
    const getData = () => {
      let id_url = url + data.find + "%22";
      axios
        .get(id_url)
        .then((result) => {
          data.message = "get ID" + data.find;
          if (result.data != null) {
            data.fire_data = result.data;
          } else {
            data.fire_data = "no data found...";
          }
        })
        .catch((error) => {
          data.message = "ERROR!";
          data.fire_data = {};
        });
    };
    onMounted(() => {
      getData();
    });
    return { data, getData };
  },
};
</script>