<template>
    <NuxtLayout name="logged"></NuxtLayout>
    <div class="mainfeed">
      <div class="navfeed">
        <div class="publicfeed">
          <h1 @onload="feedpublic">DOG API</h1>
        </div>
      </div>
      <div class="feedpost" v-html="feedpostDiv"></div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        feedpostDiv: '',
      };
    },
    mounted() {
      this.feedpublic(); // Call the feedpublic method when the component is mounted
    },
    methods: {
      feedpublic() {
        const options = {
          method: 'GET',
          url: 'https://api.thedogapi.com/v1/images/search?limit=5', // Use the DOG API URL here
          headers: {
            'x-api-key': 'YOUR_API_KEY', // Add your API key here
          },
        };
  
        axios
          .request(options)
          .then((response) => {
            const articles = response.data;
            let feedpostDiv = '';
  
            articles.forEach((article) => {
              const breedName = Array.isArray(article.breeds) && article.breeds.length > 0 ? article.breeds[0].name : 'Unknown Breed';
              const breedDescription = Array.isArray(article.breeds) && article.breeds.length > 0 ? article.breeds[0].description : 'No description available';
  
              feedpostDiv += `
                <div class="postblog" style="display: flex;border: 1px solid; padding-bottom: 10px; min-height: 200px;">
                  <div class="photopostblog" style="background-size: cover; background-position: center; transition: transform .2s; width: 40%; overflow: hidden;">
                    <img src="${article.url}" />
                  </div>
                  <div class="textpostblog" style="padding-left: 5px; background-size: cover; background-position: center; transition: transform .2s; width: 60%; ">
                    <h2><strong>${breedName}</strong></h2>
                    <p>${breedDescription}</p>
                  </div>
                </div>`;
            });
  
            this.feedpostDiv = feedpostDiv; // Update feedpostDiv
          })
          .catch((error) => {
            console.error(error);
          });
      },
    },
  };
  </script>
  
  <style scoped>
  .mainfeed {
    display: flex;
    flex-direction: column;
    /* align-items: center; */
    background-color: azure;
    justify-content: center;
    width: 70%;
    margin: 0 auto;
    
  }
  .navfeed{
      display: flex;
    flex-direction: row;
    /* align-items: center; */
    background-color: azure;
    justify-content: center;
    width: 100%;
    margin: 0 auto;
    border: 1px solid;
  }
  .followingfeed {
      display: flex;
    position: relative;
    background-color: aquamarine;
    height:fit-content;
    width: 50%;
    justify-content: center;
    /* Add desired styles */
  }
  .publicfeed {
      display: flex;
      position: relative;
      background-color: azure;
      height:fit-content;
      width: 50%;
      justify-content: center;
    /* Add desired styles */
  }
  .feedpost{
      display:block;
      position: relative;
      background-color: azure;
      height:fit-content;
      justify-content: center;
      flex-direction: row;
      padding-top: 10px;
  }
  .postblog{
      display: flex;
      padding-bottom: 10px;
      max-height: 200px;
  }
  .photopostblog{
      background-size: cover;
      background-position: center;
      transition: transform .2s;
      width: 40%;
      overflow: hidden;
  }
  .textpostblog{
      background-size: cover;
      background-position: center;
      transition: transform .2s;
      width: 60%;
      overflow: hidden;
  }
  </style>
  