<template>
<div class="admin">
  <h1>Upload Your Own Story!</h1>
  <div class="heading">
  </div>
  <div class="add">
  <div class="form">
    <h2>Write a title and upload a photo for your story!</h2>
    <input v-model="title" placeholder="Story Title">
    <input type="file" name="photo" @change="fileChanged">
    <p></p>
    <textarea v-model="story" class="form-control" id="exampleFormControlTextarea1" rows="6" cols="46" placeholder="Write Your Story Here!"></textarea>
    <p></p>
    <input v-model="suggestion" placeholder="Write Your Name Here!">
    
    <button @click="upload">Upload</button>
  </div>
  <div class="upload" v-if="addItem">
    <h2>Title: {{addItem.title}}</h2>
    <img :src="addItem.path" />
    <h2>Story: {{addItem.story}}</h2>
    <h2>Author: {{addItem.suggestion}}</h2>
    <h2>Date of Submission: {{addItem.time}}</h2>
  </div>
  </div>
  <div class="heading">
  <h1>Edit/Delete Existing Stories :(</h1>
  </div>
  <div class="edit">
    <div class = "row">
      <div class="form">
        <input v-model="findTitle" placeholder="Search by Title">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
      </div>
      <div class="form">
        <input v-model="findTitle" placeholder="Search by Author">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.suggestion}}
          </div>
        </div>
      </div>
    </div>
    <div class="form">
      <input v-model="findTitle" placeholder="Search by Date">
      <div class="suggestions" v-if="suggestions.length > 0">
        <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.time}}
        </div>
      </div>
    </div>
    <div class="upload" v-if="findItem">
      <input v-model="findItem.title">
      <p></p>
      <textarea v-model="findItem.story"></textarea>
      <p></p>
      <input v-model="findItem.suggestion">
      
      <img :src="findItem.path" />
    </div>
    <div class="actions" v-if="findItem">
      <button @click="editItem(findItem)">Save Changes</button>
      <p></p>
      <button @click="deleteItem(findItem)">Delete Story :(</button>
    </div>
  </div>

</div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'Admin',
    data() {
      return {
        title: "",
        file: null,
        story: "",
        time: "",
        addItem: null,
        items: [],
        findTitle: "",
        findAuthor: "",
        findItem: null,
        suggestion: "",
      }
    },
    computed: {
      suggestions() {
        //if you have time fix this if not then whatever
        let items = this.items//.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
        return items.sort((a, b) => a.title > b.title);
      },
    },

    created() {
      this.getItems();
    },

    methods: {
      fileChanged(event) {
        this.file = event.target.files[0]
      },
      selectItem(item) {
        this.findTitle = "";
        this.findItem = item;
      },

      async upload() {
        try {
          const formData = new FormData();
          console.log(this.story);
          let today = new Date();
          let dd = today.getDate();
          let mm = today.getMonth()+1;
          let yyyy = today.getFullYear();
          let temptime = mm.toString()+"-"+dd.toString()+"-"+yyyy.toString();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/items', {
            title: this.title,
            path: r1.data.path,
            suggestion: this.suggestion,
            story: this.story,
            time: temptime,
          });

          this.addItem = r2.data;
        } catch (error) {
          console.log(error);
        }
      },
      async getItems() {
        try {
          let response = await axios.get("/api/items");
          this.items = response.data;
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      async deleteItem(item) {
        try {
          await axios.delete("/api/items/" + item._id);
          this.findItem = null;
          this.getItems();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      async editItem(item) {
        try {
          await axios.put("/api/items/" + item._id, {
            title: this.findItem.title,
            suggestion: this.findItem.suggestion,
            story: this.findItem.story,
            time: this.findItem.time,
          });
          this.findItem = null;
          this.getItems();
          return true;
        } catch (error) {
          console.log(error);
        }
      },


    },


  }
</script>


<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}



/* Form */
input,
textarea,
select,
button {
  margin-right: 20px;
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 0px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
  text-align: center;
  inline-size: 500px;
  overflow-wrap: break-word;
}

.upload img {
  max-width: 100px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

</style>
