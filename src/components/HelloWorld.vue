<template>
  <div class="wrap">
    <h1>{{ msg }}</h1>
    <input type="text" v-model="searching" />
    <button v-on:click="showForm()">Add artist</button>
    <div class="add-info" v-if="shown" >
      <input v-model="$v.name.$model" placeholder="Artist Name"  v-on:blur="$v.name.$touch()"/><br />
      <div v-if="$v.name.$dirty && !$v.name.required">Name is required</div>
      <input v-model="$v.review.$model" placeholder="About artist" v-on:blur="$v.review.$touch()"/><br />
      <div v-if="$v.review.$dirty && !$v.review.required">The review of artist is required</div>
      <input v-model="imageUrl" placeholder="Image url" /><br />
      <button  v-on:click="addArtist()" :disabled="!$v.name.required ||!$v.review.required">Add</button>
    </div>
    <div v-if="shownLists " class="artists">
      <div
        v-for="(item,index) in filteredArtists"
       :key="index"
        class="artist-card"
      >
        <img v-bind:src="item.imageUrl" alt="" />
        <div>
          <p class="name">{{ item.name }}</p>
          <p>{{ item.review }}</p>
        </div>
        <button v-on:click="removeArtist(index)" class="delete">
          Delete
        </button>
      </div>
    </div>
  </div>
</template>
<script>
import {required} from 'vuelidate/lib/validators'
export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Artist Directory",
      searching: "",
      shown: false,
      name: "",
      review: "",
      imageUrl: "",
      artists: [],
      shownLists: false,
    };
  },
  methods: {
    showForm: function () {
      this.shown = true;
      this.shownLists = false;
    },
    addArtist: function () {
      this.shown = false;

      let addedArtist = {
        ...addedArtist,
        name: this.name,
        review: this.review,
        imageUrl: this.imageUrl,
      };
      this.artists = [...this.artists, addedArtist];
      this.shownLists = true;
      console.log(this.artists);
      this.name = '';
      this.review = '';
      this.imageUrl = '';
  
    },
    removeArtist: function (i) {
      this.artists.splice(i, 1);
    },
  },
  validations: {
    name:{
      required
    },
    review:{
      required
    },
  },
  computed: {
    filteredArtists() {
      if (!this.searching) {
        return this.artists;
      } else {
        return this.artists.filter((item) => {
          return (
            item.name.toLowerCase().indexOf(this.searching.toLowerCase()) > -1
          )
        })
      }
    }
  }
};
</script>
<style scoped>
.wrap{
  max-width: 500px;
  margin:0 auto;
}
button,
input {
  padding: 6px 10px;
  margin-bottom: 10px;
  margin-right: 10px;
}
.add-info input {
  width: 240px;
  text-align: center;
}
.artist-card {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  width:400px;
  justify-content: space-between;
  border: 1px solid grey;
  margin: 0 auto;
  margin-bottom: 10px;
  padding: 5px 20px;
}

.name{
  font-weight: bold;
  font-size: 20px;
}
.delete{
  background: red;
}
</style>
/* eslint-disable */