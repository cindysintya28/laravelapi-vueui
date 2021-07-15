<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">ADD FRIEND</h5>
      <form class="row g-3" @submit.prevent="store">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">NAMA</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="friend.nama"
          />
          <div class="alert alert-ganger" v-if="validation.nama">
            {{ validation.nama[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">NO TLP</label>
          <input
            type="number"
            class="form-control"
            id="inputPassword4"
            v-model="friend.no_tlp"
          />
          <div class="alert alert-ganger" v-if="validation.no_tlp">
            {{ validation.no_tlp[0] }}
          </div>
        </div>
        <div class="col-6">
          <label for="inputAddress" class="form-label">ALAMAT</label>
          <input
            type="text"
            class="form-control"
            id="inputAddress"
            placeholder="Masukan Alamat"
            v-model="friend.alamat"
          />
          <div class="alert alert-danger" v-if="validation.alamat">
            {{ validation.alamat[0] }}
          </div>
        </div>
         <div class="col-6">
           <label for="inputAddress" class="form-label">Group</label>
        <select class="form-select" aria-label="Default select example"
         v-model="friend.groups_id"
         >
  <option v-for="group in groups" :key="group.id" :value="group.id">
    {{ group.name }}
    </option>
</select>
</div>
        <div class="col-12">
          <button type="submit" class="btn btn-primary">ADD</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const friend = reactive({
      nama: "",
      no_tlp: "",
      alamat: "",
      groups_id: ""
    });

    let groups = ref([]);
    const validation = ref([]);

    const router = useRouter();

onMounted(() =>{
axios
        .get("http://127.0.0.1:8000/api/groups")
        .then((response) => {
          groups.value = response.data.data;
          console.log(groups.value);
        })
        .catch((error) => {
        console.log(error);
        });
  
});

    function store() {
      let nama = friend.nama;
      let no_tlp = friend.no_tlp;
      let alamat = friend.alamat;
      let groups_id = friend.groups_id;
      axios
        .post('http://127.0.0.1:8000/api/friends', {
          nama: nama,
          no_tlp: no_tlp,
          alamat: alamat,
          groups_id: groups_id
        })
        .then(() => {
          router.push({
            name: "Home",
          });
        })
        .catch((error) => {
         validation.value = error.response.data;
        });
    }
    return {
      friend,
      validation,
      router,
      store,
      groups,
    };
  },
};
</script>
