<template>
  <div class="experimentos">
    <!-- <b-button class="item" v-on:click="pegar_dados" variant="success">Pegar dados</b-button> -->
    <div class="container">
    <div class="item">
      <h1>Tutorial pannel</h1>
      <h2>Welcome to the Experiment Database Area</h2>
      <h3>Step 1:Create new database</h3>
        <p>
            Use this page to create new databases. If you don't have any databases registered, or if you want to register a new one, go to Step 1 and click "Add New Database." To manage an existing database, go to the database card and click the "Manage" button.
        </p>
        <h3>Step 2:Manage database information</h3>
        <p>
            In the Database Management area, you can upload the data produced by EthoWatcher OS (see instructions for obtaining a copy), as well as add metadata information (e.g., the gender of the animal, treatment type, etc.).
        </p>
        <h3>Step 3: Generate subsets of the Database</h3>
        <p>
            Once you've finished adding and editing data for the respective database, you can move to the Query area by clicking the "Query" button on the relevant database card. This section allows you to perform searches on the database, using the information you've registered. For example, you can select data such as the durations of immobility behavior in male or female rats treated with 2.5 mg of fluoxetine.
        </p>
        <p>
            To delete a database, click the "Delete" button—but be careful, this action is irreversible.
        </p>
    </div>
    <div>
      <div class="item">
        <h2>Step 1: Add new experiment</h2>
        <p>
          Fill the experiment name in the form bellow.
          <input
            type="text"
            placeholder="Experiment name"
            id="experiment_name"
            v-model="this.new_experiment_text"
          />
          Finally click :
          <input
            type="button"
            v-on:click="create_new_experiment()"
            value="Add New Database."
          />
        </p>
      </div>
    </div>
  </div>
    <div>

      
      <h2> Behavior Databases</h2>
      <div class="container">
        <div class="item" v-for="item in lis_experimentos" :key="item.message">
          
          <div class="card-section">
            <h2>The {{ item.nome_banco_experimental }} database</h2>
          <img
            class="img-card"
            src="@/assets/data_image.png"
            alt=""
            srcset=""
          />

          </div>
          <div class="card-section">
            <h2>Manage the database</h2>
          <p>
            <!-- Clique no botão -->
            <router-link
              v-bind:to="{ name: 'db_management', params: { hash: item._id } }"
            >
              <button id="myButton" class="foo bar">
                Step 2: go to management area
                <!-- Manage the database {{ item.nome_banco_experimental }} database -->
              </button>
            </router-link>
            Inside this area you will be able to add and edit information of this database.
            <!-- para acessar a área de gestão dos dados desse experimento, nessa
            sessão é possivel enviar os dados produzidos pelo EthoWatcher OS
            (see how to get one copy), e adicionar informações a respeito dos
            seus metadados (e.g. sexo do animal, tratamento) -->
          </p>

          </div>
          


          <div class="card-section">
            <h2>Query the Database</h2>
              <p>
                <!-- Clique no botão -->

                <router-link
                  v-bind:to="{ name: 'gestaoexp', params: { hash: item._id } }"
                >
                  <button id="myButton" class="foo bar">
                    Step 3 :query the database 
                    <!-- {{ item.nome_banco_experimental }} Database -->
                  </button>
                </router-link>
                In this area you will be able to select especific behavior descriptors of some experimental group.
                <!-- para acessar a área de gestão dos dados desse experimento. Nessa
                seção é possivel realizar pesquisas no banco de dado produzido a
                partir das informações registradas (ex. selecionar as durações do
                comportamento de imobilidade dos ratos machos ou fêmeas tratadas com 2.5mg de fluoxetina)  -->
              </p>

              <input class="warning"
                type="button"
                v-on:click="delete_experimento(item._id)"
                value="Delete Me"
              />
          </div>
          

          <!-- <b-button v-bind:href='"/gestaoexp/"+item._id' variant="primary">{{ item.nome_banco_experimental }}</b-button> -->
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import { get_url } from '../services/url'

export default {
  data() {
    return {
      new_experiment_text: "",
      r_logado: false,
      lis_experimentos: [],
    };
  },
  mounted() {
    // console.log("rodou antes do componeten")
    // let data =  localStorage.getItem("access_token");
    // let vue_component = this
    acessando_protegido(this);

    // let r_exite_toke_logado = data != undefined
    // if (r_exite_toke_logado){
    //     this.r_logado = true
    // }
    // this.$emit("update_usuario", this.r_logado)
  },

  methods: {
    create_new_experiment() {
      create_experiment(this);
      this.new_experiment_text = "";
    },
    delete_experimento(experimento_hash) {
      del_experiment(this, experimento_hash);
    },
    pegar_dados() {
      acessando_protegido(this);
    },
  },
};

function create_experiment(vue) {
  async function fun_asincrona(vue) {
    let params = {
      nome_banco_experimental: vue.new_experiment_text,
    };

    let data = localStorage.getItem("access_token");

    // this = vue_component
    // vue_component.r_logado = true
    const res = await fetch(get_url() + "/api/add_new_experimento", {
      method: "POST",
      // credentials: "include",
      headers: new Headers({
        "content-type": "application/json",
        Authorization: "Bearer " + data,
      }),
      body: JSON.stringify({
        nome_banco_experimental: params.nome_banco_experimental,
      }),
    });

    const data2 = await res.json();

    // vue.list_juncoes = data2;

    // console.log(vue.list_juncoes);
    acessando_protegido(vue);
  }

  fun_asincrona(vue);
}

function del_experiment(vue, hash) {
  async function fun_asincrona(vue) {
    let params = {
      hash: hash,
    };

    let data = localStorage.getItem("access_token");

    // this = vue_component
    // vue_component.r_logado = true
    const res = await fetch( get_url()+ "/api/delete_experimento", {
      method: "POST",
      // credentials: "include",
      headers: new Headers({
        "content-type": "application/json",
        Authorization: "Bearer " + data,
      }),
      body: JSON.stringify({
        experimento_hash: params.hash,
      }),
    });

    const data2 = await res.json();

    // vue.list_juncoes = data2;

    // console.log(vue.list_juncoes);
    acessando_protegido(vue);
  }

  fun_asincrona(vue);
}

function acessando_protegido(vue_component) {
  const pega_dados_async = async (vue_component, as) => {
    let data = localStorage.getItem("access_token");
    let j_obc_config = {
      method: "GET",
      // credentials: "include",
      headers: new Headers({
        // "content-type": "application/json",
        Authorization: "Bearer " + data,
      }),
    };

    let res = await fetch(
      get_url()+ "/api/get_experimentos_user",
      j_obc_config
    );
    let data_r = await res.json();
    vue_component.lis_experimentos = data_r["experimentos"];
    console.log(data_r);
  };
  pega_dados_async(vue_component, 192);
}
</script>


<style scoped>
.img-card {
  max-width: 30%;
}
.container {
  display: flex;
  flex-wrap: wrap;
}
.item {
  font-size: 1.1em;
  margin: 20px;
  border-style: solid;
  border-width: medium;
  padding: 20px;
}
.experimentos {
  margin: 80px;
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

.card-section{
  border-top-style: ridge;
  margin-top: 20px;
  margin-bottom: 20px;
}
.warning{
  background: rgb(255, 105, 130);
}
</style>