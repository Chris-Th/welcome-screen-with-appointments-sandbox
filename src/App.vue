<template>
  <!-- ===========================
  <img alt="Vue logo" src="./assets/logo.png">
  ============================ -->
  <!-- <HelloWorld msg=""/> -->
  
  <div id="app">
    <div id="main-frame">
      <div id="main-container" class="flex-container"> 
        
        <div class="header">
          <!-- Main title -->
          <div class="top-title">
            <h1>
              {{ title }}
            </h1>
          </div>
          <!-- Current date subtitle -->
          <h1 class="date">
            {{ currentDate }}
          </h1>
        </div>
        <div class="entries-container">
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
      <div class="footer">
        <img :src="logoSTZH" alt="Logo STZH"  style="width: 260px;" class=""> 
        <img :src="logOpp" alt="Logo Opportunity" style="width: 236px;" class=""> 
        <img :src="logoSAG" alt="Logo SAG" style="width: 273px;" class=""> 
      </div>
    </div>
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
    sheet_id: "1eYj2fR4zENBDj44B_2eNcLb9q5nw5jyuhQlhweDottM",
    api_token: "AIzaSyDj-0jsLeXGGG7krtGwhojubzhsjKZ4rOI",
    entries: [],
    currentDate: "",
    logoSTZH: require('./assets/STZH_SEB_Logo.png'),
    logOpp: require('./assets/Opportunity.png'),
    logoSAG: require('./assets/SAG_Logo_De.png')
  };
},

// My API-Key: AIzaSyDj-0jsLeXGGG7krtGwhojubzhsjKZ4rOI
// Project Name on console.cloud.google.com: "Welcome Screen Opportunity"
// https://docs.google.com/spreadsheets/d/1eYj2fR4zENBDj44B_2eNcLb9q5nw5jyuhQlhweDottM/edit?usp=share_link
 

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
     
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap');


 * {
   font-family: Inter, sans-serif, Helvetica;
   box-sizing: border-box;
 }

 #app {
  background-color: #ffffff;
 }
 
  #main-frame {
    width: 1080px;
    height: 1920px;
    margin: auto;
  }

 .flex-container {
  display: flex;
  align-items:flex-start;
  width: 100%;
  height: 100%;
  justify-content: flex-start;
  flex-direction: column;
  align-content: flex-start;
  padding-inline: 60px;
  margin: auto;
  
}

#main-container {
  /*background-color: #e8eff4b7;*/
  background-color: #E8EFF4;
}

.header {
  background-color: #E8EFF4;
  position: sticky;
    top: 0;
    margin: 0;
    width: 100%;
}

.entries-container {
   /*display: inline-block;*/
   display: flex;
   flex-direction: column;
   background-color: inherit;
   width: 100%;
   height: fit-content;
   margin-inline: auto;
   padding-block: 1rem;
   padding-inline: auto;
 }

 ul.entries-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
 }

.entry-item {
   /*display: inline-block;*/
   background-color: #0F05A0;
   width: 100%;
   height:fit-content;
   margin-inline: auto;
   margin-bottom: 40px;
   padding-block: 2rem;
   padding-inline: 2rem;
   /*margin: 0 3rem 1.5rem;*/
 }



 .top-title > h1 {
   color: #323D4A;
   font-weight: 900;
   font-size: 62px;
   line-height: 75px;
   margin-bottom: 0;
   margin-top: 50px;
 }

 .date {
   color: #9AA7B1;
   font-size: 62px;
   line-height: 72px;
   font-weight: 500;
   font-style: normal;
   display: block;
   margin-block: 21px;
 }

 .item {
   
   font-size: 28px;
   line-height: 36px; 
   margin-bottom:0;
   margin-top: 5px;
 }
 
 .item-time {
   color: #EB5E00;
   font-weight: 900;
   padding:0;
 }
 .item-title {
   color: #FFBFAB;
   font-weight: 900;
 }
 .item-description {
   color: #FFBFAB;
   font-weight: 500;
 }

 /* 
 display: flex;
  
  width: 1080px;
  height: 1920px;
  
  
  align-content: flex-start;
  
  margin: auto;
 */

 .footer {
    display: flex;
    align-items:center;
    flex-direction: row;
    justify-content: space-between;
    padding-inline: 40px;
    position: sticky;
    bottom: 0;
    margin: 0;
    width: 100%;
    height: 130px;
    background-color: white;
 }

 .logo {
   height: 55px;
 }
</style>
