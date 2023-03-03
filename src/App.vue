<template>
  <!-- ===========================
  <img alt="Vue logo" src="./assets/logo.png">
  ============================ -->
  <HelloWorld msg=""/>
  
  <div id="app">
  
    <div id="main-container" class="flex-container">      
      <h1 class="top-title pt-3">
        {{ title }}
      </h1>
      <h1 class="date">
        {{ currentDate }}
      </h1>
      <div class="entry-item">
        <ul v-if="entries && entries.length" class="entries-list" >
          <li  class="entry-item" v-for="entry in entries" :key="entry.id">
            <p class="item item-time">{{ entry[0] }} Uhr, {{ entry[1].replaceAll("/", ".") }} </p>
            <p class="item item-title">{{ entry[2] }}</p>
            <p class="item item-description">{{ entry[3] }}</p>
          </li>
        </ul>
        <h1 v-else>Zur Zeit keine Events!</h1> 
      </div>
    </div>

    <nav class="navbar navbar-expand-sm fixed-bottom footer py-0">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">
          <img src="img/STZH_SEB_Logo.png" alt="Avatar Logo"  style="width: 230px;" class=""> 
        </a>
        <a class="navbar-brand" href="#">
          <img src="img/Opportunity.png" alt="Avatar Logo" style="width: 200px;" class=""> 
        </a>
        <a class="navbar-brand" href="#">
          <img src="img/SAG_Logo_De.png" alt="Avatar Logo" style="width: 200px;" class=""> 
        </a>
      </div>
    </nav>  

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
// import { time } from 'console';

import axios from "axios"; // axios is a library for making HTTP requests to the backend

export default {
  name: 'App',
  data() {
	return {
    title: "Welcome to Opportunity",
    sheet_id: "1CR1UKN0LAPNs6lWbfA2gBI2FazmWdVSFIzIwi5TG5Z4",
    api_token: "AIzaSyA-qeDXOhEeQDA0vQf7LgkF7DQtGnAtmAU",
    entries: [],
    currentDate: "",
  };
},

 

  computed: {
    gsheet_url() {
  return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
  },
},


  methods: {
  /*  getData() {
      this.entries = [ 
        // time, date, title, description
        ["8.25", "11/11/1111", "Feier", "failure"],
        ["17.25", "22/22/2222", "wedding", "of a paper clip"]
      ] 
    },*/

    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },

    
    updateCurrentDate() {
      let today = new Date();
      this.counter++;
      this.currentDate = `${today.getDate()}.${today.getMonth()}.${today.getFullYear()}`;
      this.currentTime = ``;

    },
    refreshData() {
      this.updateCurrentDate();
      this.getData();
    },
  },
  mounted() {
    this.refreshData(); // get first initial data then wait for the next
    setInterval(this.refreshData, 18000000); // wait 1 min for next update
  },
};




  /*components: {
    HelloWorld
  }
};*/
</script>

<style>
     
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap%22");

 * {
   font-family: inter;
   box-sizing: border-box;
 }
 
 .flex-container {
  display: flex;
  align-items: center;
  width: 1080px;
  height: 1920px;
  justify-content: center;
  
}

.main-container {
  background-color: #E8EFF4;
}


 .top-title {
   color: #323D4A;
   font-weight: 900;
   font-size: 62px;
   line-height: 75px;
   margin-bottom: 0.8rem;
 }

 .date {
   color: #9AA7B1;
   font-size: 62px;
   line-height: 72px;
   font-weight: 100;
   font-style: normal;
   display: block;
 }

 .item {
   
   font-size: 28px;
   line-height: 36px; 
   margin-bottom:0.1rem;
 }
 .entry-item {
   display: inline-block;
   background-color: #0F05A0;
   width: 100%;
   height:fit-content;
   margin: auto;
   padding-block: 2rem;
   padding-inline: 2rem;
   margin: 0 3rem 1.5rem;
 
 }
 .item-time {
   color: #EB5E00;
   font-weight: bold;
   padding:0;
 }
 .item-title {
   color: #FFBFAB;
   font-weight: bolder;
 }
 .item-description {
   color: #FFBFAB;
 }

 .footer {
   width: 100%;
   height: 130px;
   padding: 0.5rem;
   background-color: white;
 }

 .logo {
   height: 55px;
 }
</style>
