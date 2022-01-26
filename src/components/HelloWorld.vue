<template>
  <div class="wrap">
    <h1>{{ msg }}</h1>
    <input type="text" v-model="searching" />
    <button v-on:click="showForm()">Add artist</button>
    <div class="add-info">
      <input v-if="shown" v-model="name" placeholder="Artist Name" /><br />
      <input v-if="shown" v-model="review" placeholder="About artist" /><br />
      <input v-if="shown" v-model="imageUrl" placeholder="Image url" /><br />
      <button v-if="shown" v-on:click="addArtist()">Add</button>
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
      this.name = "";
      this.review = "";
      this.imageUrl = "";
  
    },
    removeArtist: function (i) {
      this.artists.splice(i, 1);
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
          );
        });
      }
    },
  },
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
