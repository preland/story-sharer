<template>
  
  <div class="container-fluid">
   <div class = "col-lg">
      <div class= "row">
        <div class = "websiteHead">Welcome to Story Sharer!</div>
      </div>
    </div>
  <div class = "col-lg">
    <div class= "row">
      <div class = "websiteSubHead">The place to share stories.</div>
    </div>
  </div>
      <div class="image" v-for="item in items" :key="item.id">
        <div class = "col-lg">
          <div class= "row">
            <h1>{{item.title}}</h1>
          </div>
          <div class= "row">
            <img :src="item.path" />
          </div>
          <div class= "row">
            <h2>{{item.story}}</h2>
          </div>
          <div class= "row">
            <p>Made by: {{item.suggestion}}</p>
          </div>
          <div class= "row">
            <p>Submission Date: {{item.time}}</p>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data() {
    return {
     items: [],
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }

}
</script>

<style scoped>
p{
  margin-top: 0px;

}
.websiteHead{
  font-size: 69px;
  padding: 30px;
}
.websiteSubHead{
  font-size: 35px;
  padding: 60px;
}
.row{
  display: flex;
  justify-content: center;
}
.home{
  align-items: center;
}
.image h2 {
  font-style: italic;
  line-height: 2.0;
  align-text: center;
  inline-size: 500px;
  overflow-wrap: break-word;
}

/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 1.5em;
}

.image {
  margin: 0 0 1.5em;
  display: flex;
  width: 100%;
  justify-content: center;
}

.image img {
  width: 20%;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
