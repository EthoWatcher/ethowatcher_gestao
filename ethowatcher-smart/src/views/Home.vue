<template>
  <div class="oi">
    <h1>Welcome to EthoWatcher data analysis plataform.</h1>
    <div class="content-main">
      <div class="content-main-item">
        
        <h2> Introduction</h2>
        <p>
           Use this graphical interface to register your experiments (behavioral data, including categorical and morpho-kinetic information) in order to create smaller datasets for data exploration, statistical modeling, and machine learning training.
          
        </p>

        <p>This software was developed by João A. Marcolan as part of his doctoral studies, under the supervision of José Marino Neto at the <a href="http://www.ieb.ufsc.br/?page_id=139">Instituto de Engenharia Biomédica</a> 
        </p>

        <!-- <h2> First step: Register an user</h2>
        <p>
           Use the follow form  to register an user 

        </p> -->
        <h1>Tutorial pannel</h1>
        <h2>Step 1. Download EthoWatcher OS and use it to collect data.</h2>

        <p>
          To get started, you'll need to gather data using the EthoWatcher OS software. You can <a href="https://github.com/EthoWatcher/ethowatcher/wiki">download a copy here and follow the tutorial.</a>
        </p>
        <p> If do you have some questions, <a href="https://github.com/EthoWatcher/ethowatcher/issues"> use our forum to ask questions.</a> </p>
        <img src="@/assets/etowatcheros.png" alt="" srcset="">
        

      </div>

      <div class="content-main-item">

        <RegistroUsuario />
        
        

      </div>
    </div>

    
  </div>
</template>


<script>
import RegistroUsuario from "../components/RegistroUsuario.vue";

export default {
  components: {
    RegistroUsuario,
  },
  setup() {},
};
</script>


<style scoped>
.content-main{
  display: flex;
  margin: 80px;

  justify-content: space-around;
}

p{
  margin: 10px;
}

img {
  display: block;
  max-width:800px;
  max-height:800px;
  width: auto;
  height: auto;
  margin: 40px;
}
</style>