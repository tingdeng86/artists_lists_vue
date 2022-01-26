<template>
  <div>
    <h1>{{ msg }}</h1>
    <input type="text" v-model="searching" />
    <button v-on:click="showForm()">Add artist</button>
    <div>
      <input v-if="shown" v-model="name" placeholder="Artist Name" /><br />
      <input v-if="shown" v-model="review" placeholder="About artist" /><br />
      <input v-if="shown" v-model="imageUrl" placeholder="Image url" /><br />
      <button v-if="shown" v-on:click="addArtist()">Add</button>
    </div>
    <div v-if="shownLists" class="artists">
      <div
        v-for="item in filteredArtists"
        v-bind:key="item.index"
        class="artist-card"
      >
        <img v-bind:src="item.imageUrl" alt="" />
        <p>{{ item.name }}</p>
        <p>{{ item.review }}</p>

        <button v-on:click="removeArtist(item.index)">Delete</button>
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
      console.log(this.filterArtists);
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
