<template>
  <div id="ListaDeJuncoes">

    <h2>Step 4: Add EthoWatcher OS data and organize they</h2>
    <div class="metadata-menu">
      

    <div class="metadata-menu-item">
      <p>
    To add a new video file and they respective file click "Add video file" button. Once you've created the new slot:
    <ul>
    <li>1. Upload the video file (.vxml).</li>
    <li>2. Upload the ethographic file corresponding to the uploaded video (.etoxml).</li>
    <li>3. Upload the tracking file associated with the uploaded video (.tracking).</li>
    <li>4. Select the metadata related to the uploaded video.</li>
    <li>5. Repeat for additional files or categories as needed.</li>
    </ul>
    </p>

    </div>

    <div class="metadata-menu-item">
      
          <input
          type="button"
          v-on:click="add_new_juncao()"
          value=" Add video file"
        /> 
  
      </div>
    
  </div>
    
  <h2> EthoWatcher OS file list</h2>
    <div v-for="(juncoes, index) in this.list_juncoes['juncoes']" :key="index">
      <div class="juncao-menu">
        <Juncao 
        v-bind:juncoes="juncoes"
        v-bind:experimento="this.experimento"
        v-on:delete_juncao="update_juncao()"
        v-if="renderComponent"
         />
      </div>
    </div>
    <br />
  </div>
</template>


<script>
import Juncao from "./Juncao.vue";
import { get_url } from '../../../services/url'

export default {
  components: {
    Juncao,
  },
  props: {
    experimento: {},
  },
  setup() {},
  data() {
    return {
      list_juncoes: [],
      experimento: this.experimento,
      var_ind: this.experimento.var_inde,
      renderComponent: true
    };
  },
  methods: {
    add_new_juncao() {
      console.log(this.experimento["_id"]);
      add_new_juncao(this, this.experimento["_id"]);
    },
    update_juncao() {
      this.renderComponent = false;
      get_lista_juncao_db(this, this.experimento["_id"]);
      this.$emit("update_list")

    }
  },
  mounted() {
    this.update_juncao()
    this.renderComponent =false
    this.renderComponent = true
  },
};

function add_new_juncao(vue, id_experimento) {
  async function fun_asincrona(vue, id_experimento) {
    let params = {
      id_experimento: id_experimento,
    };

    let data = localStorage.getItem("access_token");

    // this = vue_component
    // vue_component.r_logado = true
    const res = await fetch( get_url()+ "/api/create_new_juncao", {
      method: "POST",
      // credentials: "include",
      headers: new Headers({
        "content-type": "application/json",
        Authorization: "Bearer " + data,
      }),
      body: JSON.stringify({
        id_experimento: params.id_experimento,
      }),
    });
    const data2 = await res.json();

    get_lista_juncao_db(vue, id_experimento);
  }

  fun_asincrona(vue, id_experimento);
}

function get_lista_juncao_db(vue, id_experimento) {
  async function fun_asincrona(vue, id_experimento) {
    let params = {
      id_experimento: id_experimento,
    };

    let data = localStorage.getItem("access_token");

    // this = vue_component
    // vue_component.r_logado = true
    const res = await fetch( get_url() + "/api/get_list_juncao", {
      method: "POST",
      // credentials: "include",
      headers: new Headers({
        "content-type": "application/json",
        Authorization: "Bearer " + data,
      }),
      body: JSON.stringify({
        id_experimento: params.id_experimento,
      }),
    });
    const data2 = await res.json();

    vue.list_juncoes = data2;
    
    vue.renderComponent = true;
    console.log(vue.list_juncoes);
  }

  fun_asincrona(vue, id_experimento);
}
</script>


<style scoped>
.juncao-menu {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
  border-top-style: ridge;
  
}

.control-menu {
  display: flex;
  gap: 100px;
}
.metadata-menu {
  display: flex;
  justify-content: space-around;
}
.metadata-menu-item {
  margin: 40px;
}
input {
  min-width: 40px;
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}

button {
  min-width: 40px;
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
</style>