<template>
  <NuxtLayout name="logged"></NuxtLayout>
  <div class="mainfeed" v-html="profileDiv"></div>
  <div class="profileeditmodal" id="profileeditbtn">
    <div id="modal" class="modal">
      <div class="modal-content">
        <span onclick="profilemodalclose()" class="close">&times;</span>
        <h2>Edit Item</h2>
        <form id="edit-form" v-on:submit.prevent="save_cred()" method="POST">
          <div>
            <label for="image-upload">Image:</label>
            <input type="file" @change="handleImageChange" accept="image/*">
          </div>
          <!-- <div>
            <label for="item-name">Name:</label>
            <input type="text" v-model="itemname">
          </div> -->
          <div>
            <label for="item-description">Description:</label>
            <textarea v-model="itemdescription"></textarea>
          </div>
          <div>
            <button type="submit">Save</button>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="profilepublishmodal" id="profilepublishbtn">
    <div id="modal" class="modal">
      <div class="modal-content">
        <span onclick="profilepublishmodalclose()" class="close">&times;</span>
        <h2>Edit Item</h2>
        <form id="edit-form" v-on:submit.prevent="save_publish_cred()" method="POST">
          <div>
            <label for="imagepublish-upload">Image:</label>
            <input type="file" @change="handleImagepublishChange" accept="image/*">
          </div>
          <div>
            <label for="itempublish-name">Name:</label>
            <input type="text" v-model="itempublishname">
          </div>
          <div>
            <label for="itempublish-description">Description:</label>
            <textarea v-model="itempublishdescription"></textarea>
          </div>
          <div>
            <button type="submit">Save</button>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="profileposteditmodal" id="profileposteditbtn">
    <div id="modal" class="modal">
      <div class="modal-content">
        <span @click="profilepostmodalclose()" class="close">&times;</span>
        <h2>Edit Item</h2>
        <form id="edit-form" v-on:submit.prevent="save_post_cred(itempostid)" method="POST">
          <div>
            <label for="imagepost-upload">Image:</label>
            <input type="file" @change="handleImagepostChange" accept="image/*">
          </div>
          <div>
            <label for="item-name">Name:</label>
            <input type="text" v-model="itempostname">
          </div>
          <div>
            <label for="item-description">Description:</label>
            <textarea v-model="itempostdescription"></textarea>
          </div>
          <div>
            <button type="submit">Save</button>
            <button>Delete</button>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="profilefeedpost mainfeed" v-html="profilefeedpostDiv" id="profilefeedpost"></div>
</template>

<script>
 import axios from 'axios';
export default {
  
  data() {

          // console.log(description);
          // console.log(profilepic_url);
    return {
      profileDiv: `
        <div class="profilediv" style="flex-direction: row; display: flex;">
          <div class="profilepic" style="background-color: azure;width: 30%;aspect-ratio: 1/1;display: flex;justify-content: center;align-items: center;flex-direction: column;position: relative;height: fit-content;">
            <div class="profilepic-img" style="width: 100%; height: 100%; background-size: cover; background-position: center; border-radius: 50%; display: flex; justify-content: center; align-items: center; overflow: hidden;">
              <img src="/${localStorage.getItem('profilepic_url')}" id="profilediv-img" value="${localStorage.getItem('profilepic_url')}" style="width: 100%; height: 100%; object-fit: cover; object-position: center; border-radius: 50%;" />
            </div>
          </div>
          <div class="profileinfo" style="flex: 1;display: flex;flex-direction: column;position: relative;background-color: azure;">
            <div class="profilename">
              <h1 id="profilediv-name" value="${localStorage.getItem('name')}"><strong>${localStorage.getItem('name')}</strong></h1>
              <div class="profilebtn">
                <button onclick="profileedit()" type="button" style="text-decoration: underline;">Edit</button>
                <button onclick="profilepublish()" type="button" style="text-decoration: underline;">Publish</button>
              </div>
            </div>
            <div class="profileabout">
              <p id="profilediv-description" value="${localStorage.getItem('description')}">${localStorage.getItem('description')}</p>
            </div>
          </div>
        </div>
      `,profilefeedpostDiv: '',
      itempostid:'',
      itemdescription:'',
      imageupload:'',
      imagepostupload:'',
      itempostname:'',
      itempostdescription:'',
      imagepublishupload:'',
      itempublishname:'',
      itempublishdescription:''
    };
  },
  mounted(){
    const editButton = document.querySelector('button[onclick="profileedit()"]');
    editButton.addEventListener('click', this.profileedit);
    const publishButton = document.querySelector('button[onclick="profilepublish()"]');
    publishButton.addEventListener('click', this.profilepublish);
    const closeButton = document.querySelector('span[onclick="profilemodalclose()"]');
    closeButton.addEventListener('click', this.profilemodalclose);
    const closepublishButton = document.querySelector('span[onclick="profilepublishmodalclose()"]');
    closepublishButton.addEventListener('click', this.profilepublishmodalclose);

    const profilefeedpost = document.getElementById('profilefeedpost');
    profilefeedpost.addEventListener('click', this.articleedit);

    // const postcloseButton = document.querySelector('span[onclick="profilepostmodalclose()"]');
    // postcloseButton.addEventListener('click', this.profilepostmodalclose);
    
    // document.querySelector('button[onclick="articleedit()"]').addEventListener('click', this.articleedit);

    // const posteditButton = document.querySelector('button[onclick="articleedit()"]');
    // posteditButton.addEventListener('click', this.articleedit);
    // const articleeditButton = document.querySelector('button[onclick="profileedit()"]');
    // document.addEventListener('click', this.articleedit);
    //////////////////////////
          const options = {
              method: 'GET',
              url: 'http://localhost:1337/api/user-articles',
              headers: {
              Authorization: 'Bearer ' + localStorage.getItem("jwt_token")
              }
          };

          axios.request(options).then((response) => {
              const articles = response.data.data;
              let profilefeedpostDiv = '';
              console.log(articles);

              articles.forEach((article) => {
                if(article.attributes.users_id == localStorage.getItem("id")){
                profilefeedpostDiv += `
                    <div class="postblog" style="display: flex; padding-bottom: 10px; min-height: 200px;">
                        <div class="photopostblog" style="background-size: cover; background-position: center; transition: transform .2s; width: 40%; overflow: hidden;">
                            <img src="/${article.attributes.image_url}" id="article-div${article.id}" value="${article.attributes.image_url}"/>
                        </div>
                        <div class="textpostblog" style="margin-left: 5px; background-size: cover; background-position: center; transition: transform .2s; width: 60%; ">
                            <h2><strong>${article.attributes.title}</strong></h2>
                            <div class="articlebtn">
                              <button value="${article.id}" @click="articleedit()" name="edit" type="button" style="text-decoration: underline;">Edit</button>
                              <button value="${article.id}" @click="articleedit()" name="delete" type="button" style="text-decoration: underline;">Delete</button>
                            </div>
                            <p>${article.attributes.description}</p>
                        </div>
                    </div>`;
                }
              });

              this.profilefeedpostDiv = profilefeedpostDiv; // Update feedpostDiv

          }).catch((error) => {
              console.error(error);
          });
          
  },
  methods:{
    save_publish_cred() {
    // Create an object with the form data
    const formData = {
      image: this.imagepublishupload ? "assets/" + this.imagepublishupload.name : '',
      name: this.itempublishname,
      description: this.itempublishdescription
    };
    // Log the form data to the console
    console.log(formData);
    
    // Additional processing or form submission can be done here
      const options = {
        method: 'POST',
        url: 'http://localhost:1337/api/user-articles',
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem("jwt_token"),
          'content-type': 'application/json'
        },
        data: {
          data: {
            description: formData.description, 
            image_url: formData.image, 
            title: formData.name, 
            users_id: localStorage.getItem('id')
          }
        }
      };

      axios.request(options).then(function (response) {
        console.log(response.data);
        // document.getElementById('profilediv-img').src = "/" + formData.image;
        // document.getElementById('profilediv-img').value = formData.image;
        // document.getElementById('profilediv-description').value = formData.description;
        // document.getElementById('profilediv-description').innerText = formData.description;
        // localStorage.setItem('profilepic_url', formData.image);
        // localStorage.setItem('description', formData.description);
        document.getElementById("profilepublishbtn").style.display= "none";
        window.open("http://localhost:3000/profile","_self");
      }).catch(function (error) {
        console.error(error);
      });
    },
    save_post_cred(id){
    // Create an object with the form data
    const formData = {
      image: this.imagepostupload ? "assets/" + this.imagepostupload.name : document.getElementById("article-div" + id).getAttribute("value"),
      title: this.itempostname,
      description: this.itempostdescription
    };
    // Log the form data to the console
    console.log(formData);
    
    // Additional processing or form submission can be done here
      const options = {
        method: 'PUT',
        url: 'http://localhost:1337/api/user-articles/' + id,
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem("jwt_token"),
          'content-type': 'application/json'
        },
        data: {data: {description: formData.description, title: formData.title, image_url: formData.image}}
      };

      axios.request(options).then(function (response) {
        console.log(response.data);
        // document.getElementById('profilediv-img').src = "/" + formData.image;
        // document.getElementById('profilediv-img').value = formData.image;
        // document.getElementById('profilediv-description').value = formData.description;
        // document.getElementById('profilediv-description').innerText = formData.description;
        // localStorage.setItem('profilepic_url', formData.image);
        // localStorage.setItem('description', formData.description);
        document.getElementById("profileposteditbtn").style.display= "none";
        window.open("http://localhost:3000/profile","_self");
      }).catch(function (error) {
        console.error(error);
      });
    },
    articleedit(event){
      console.log(event.target.value)

      if(event.target.name == 'edit'){
        console.log(event.target.name)
        document.getElementById("profileposteditbtn").style.display= "flex";
        // this.itemname = document.getElementById("profilediv-name").getAttribute("value");
        // this.itemdescription = document.getElementById("profilediv-description").getAttribute("value");
        const options = {
                method: 'GET',
                url: 'http://localhost:1337/api/user-articles/' + event.target.value ,
                headers: {
                Authorization: 'Bearer ' + localStorage.getItem("jwt_token")
                }
            };

            axios.request(options).then((response) => {
              const data = response.data.data.attributes;
              this.itempostname = data.title;
              this.itempostdescription = data.description;
              this.itempostid = event.target.value;
            }).catch((error) => {
                console.error(error);
            });
      }else if(event.target.name == 'delete'){
        console.log(event.target.name)
        const options = {
          method: 'DELETE',
          url: 'http://localhost:1337/api/user-articles/'+event.target.value,
          headers: {
            Authorization: 'Bearer '+ localStorage.getItem('jwt_token'),
            'content-type': 'application/json'
          },
          data: {id: event.target.value}
        };

        axios.request(options).then(function (response) {
          console.log(response.data);
          window.open("http://localhost:3000/profile","_self");
        }).catch(function (error) {
          console.error(error);
        });
      }
    },
    profilepostmodalclose(){
      document.getElementById("profileposteditbtn").style.display= "none";
    },
    profileedit(){
      console.log("Edit");
      document.getElementById("profileeditbtn").style.display= "flex";
      console.log(document.getElementById("profilediv-img").getAttribute("value"));
      // console.log(document.getElementById("profilediv-name").getAttribute("value"));
      // console.log(document.getElementById("profilediv-description").getAttribute("value"));
      this.itemname = document.getElementById("profilediv-name").getAttribute("value");
      this.itemdescription = document.getElementById("profilediv-description").getAttribute("value");
    },
    profilepublish(){
      console.log("Publish");
      document.getElementById("profilepublishbtn").style.display= "flex";
      // console.log(document.getElementById("profilediv-img").getAttribute("value"));
      // // console.log(document.getElementById("profilediv-name").getAttribute("value"));
      // // console.log(document.getElementById("profilediv-description").getAttribute("value"));
      // this.itemname = document.getElementById("profilediv-name").getAttribute("value");
      // this.itemdescription = document.getElementById("profilediv-description").getAttribute("value");
    },
    profilemodalclose(){
      console.log("Close");
      // console.log(this.imageupload.name);
      // this.itemname = '';
      // this.itemdescription = '';
      // this.imageupload.name = '';
      document.getElementById("profileeditbtn").style.display= "none";
    },
    profilepublishmodalclose(){
      console.log("Close");
      // console.log(this.imageupload.name);
      // this.itemname = '';
      // this.itemdescription = '';
      // this.imageupload.name = '';
      document.getElementById("profilepublishbtn").style.display= "none";
    },
    handleImageChange(event) {
      console.log("hey");
      this.imageupload = event.target.files[0];
    },
    handleImagepublishChange(event) {
      console.log("hey");
      this.imagepublishupload = event.target.files[0];
    },
    handleImagepostChange(event) {
      // console.log("handleImagepostChange ");
      // console.log(event.target.files[0]);
      this.imagepostupload = event.target.files[0];
    },
    save_cred() {
    // Create an object with the form data
    const formData = {
      image: this.imageupload ? "profilepic/" + this.imageupload.name : document.getElementById("profilediv-img").getAttribute("value"),
      // name: this.itemname,
      description: this.itemdescription
    };
    // Log the form data to the console
    console.log(formData);
    
    // Additional processing or form submission can be done here
      const options = {
        method: 'PUT',
        url: 'http://localhost:1337/api/user-profiles/' + localStorage.getItem("userprofile_id"),
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem("jwt_token"),
          'content-type': 'application/json'
        },
        data: {data: {description: formData.description, profilepic_url: formData.image}}
      };

      axios.request(options).then(function (response) {
        console.log(response.data);
        document.getElementById('profilediv-img').src = "/" + formData.image;
        document.getElementById('profilediv-img').value = formData.image;
        document.getElementById('profilediv-description').value = formData.description;
        document.getElementById('profilediv-description').innerText = formData.description;
        localStorage.setItem('profilepic_url', formData.image);
        localStorage.setItem('description', formData.description);
        document.getElementById("profileeditbtn").style.display= "none";
      }).catch(function (error) {
        console.error(error);
      });
    },
    // profilepostmodalclose(){
    //   console.log("Close");
    //   // console.log(this.imageupload.name);
    //   // this.itemname = '';
    //   // this.itemdescription = '';
    //   // this.imageupload.name = '';
    //   // document.getElementById("profileposteditbtn").style.display= "none";
    //   // document.addEventListener('onclick', this.handleArticleEditClick);
    //   this.modaldisplay("none");
    // },
    // handleArticleEditClick(event) {
    //   console.log(event.target.value)
    //   // document.getElementById("profileposteditbtn").style.display= "flex";
    //   // const target = event.target;
    //   // if (target.tagName === 'BUTTON' && target.getAttribute('data-action') === 'article-edit') {
    //   //   const articleId = target.getAttribute('data-article-id');
    //   //   this.articleedit(articleId);
    //   // }
    //   // document.removeEventListener('click', this.handleArticleEditClick);
    //   this.modaldisplay("flex")
    // },
    // modaldisplay(data){
    //   console.log(data)
    //   document.getElementById("profileposteditbtn").style.display= data;
    // },
    
  }
  
}
</script>

<style scoped>
.profileeditmodal{
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.profilepublishmodal{
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.profileposteditmodal{
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.mainfeed {
  display: flex;
  flex-direction: column;
  background-color:azure;
  justify-content: center;
  width: 70%;
  margin: 0 auto;
  border: 1px solid;
}

.profilediv {
  /* height: 20rem; */
  flex-direction: row;
  display: flex;
}

.profilepic {
  background-color: azure;
  width: 30%;
  aspect-ratio: 1/1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: relative;
  height: fit-content;
}

.profilepic-img {
width: 70%;
height: 70%;
background-size: cover;
background-position: center;
border-radius: 50%;
display: flex;
justify-content: center;
align-items: center;
}
.pofileinfo {
flex: 1;
display: flex;
flex-direction: column;
position: relative;
background-color: aqua;
}
.profilefeedpost{
  padding-top: 20px;
}
/* ////////////////// */
/* Modal Styles */
.modal {
display: flex;
position: fixed;
z-index: 1;
left: 0;
top: 0;
width: 100%;
height: 100%;
overflow: auto;
background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
background-color: #fefefe;
margin: 10% auto;
padding: 20px;
border: 1px solid #888;
max-width: 500px;
}

.close {
color: #aaa;
float: right;
font-size: 28px;
font-weight: bold;
cursor: pointer;
}

.close:hover,
.close:focus {
color: black;
text-decoration: none;
cursor: pointer;
}

/* Form Styles */
form {
margin-top: 20px;
}

label {
display: block;
margin-bottom: 5px;
}

input[type="text"],
textarea {
width: 100%;
padding: 5px;
border: 1px solid #ccc;
}

button[type="submit"] {
padding: 10px 20px;
background-color: #4caf50;
color: white;
border: none;
cursor: pointer;
}

button[type="submit"]:hover {
background-color: #45a049;
}
</style>
