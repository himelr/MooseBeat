<template>
  <div class="album">

    <div id="lyricTab">
      <!-- Tabs with card integration -->
      <b-card>
        <h2>Song lyrics: {{$route.params.title}}</h2>
        <br>

        <b-tabs small card ref="tabs" v-model="tabIndex">

          <b-card>
          <pre>
          {{this.lyricData}}
          </pre>
            </b-card>

          <div v-for="data in trackData" :key="data">

            <b-tab v-for="data2 in data.media[0].tracks" :key="data2" v-on:click="say(data.artist,data2.title)" :title="data2.title">

    
            
            </b-tab>

          </div>

        </b-tabs>

        <span class="text-muted">Current Tab: {{tabIndex + 1}}</span>
      </b-card>

      <!-- Control buttons-->

    </div>
  </div>

  <!-- <p>{{this.lyricData}} </p>
      <ul>
        <li v-for="data in trackData" :key="data">
          <div class="albumPic">
            <img class="albumPicture" :src="data.images[0].url + '?user_key=468c1cfb7b96f816544e86fa0698b0cd&inc=images&maxResultCount=1'" />
          </div>
          <div v-for="data2 in data.media[0].tracks" :key="data2">

            <p> {{data2.title}} </p>
            <button v-on:click="say(data.artist,data2.title)">Get lyrics</button>

          </div>
        </li>

      </ul> -->

  <!-- <a :href='data.url' target="_blank">
                                                    <img class="albumPicture" :src='data.picture' alt='img' />
                                                  </a> -->

  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'album',
  data() {
    return {
      albumData: '',
      trackData: '',
      lyricData: '',
      abIndex: 1,
    }
  },
  created: function() {
    axios.get("/api/albums/get/" + this.$route.params.artist + "/" + this.$route.params.title)
      .then((response) => {
        this.trackData = response.data;
        this.albumData = response.data;

      })
      .catch(function(error) {

      });
  },
  methods: {
    say: function(artist, title) {
      axios.get("/api/track/get/lyrics/" + artist + "/" + title)
        .then((response) => {

          this.lyricData = response.data.lyric;

        })
        .catch(function(error) {

        });

    }


  }

}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.albumPic {
  border-radius: 4px;
  border: 5px solid #1d2120;
  height: 15em;
  width: 15em;
  display: block;
  margin: auto;
  margin: 1em;
}



.albumPicture {
  width: 100%;
  height: 100%;
  max-width: 100%;
  max-height: 100%;
}

#lyricTab {
  width: 90%;
  margin-top: 5em;
  margin-bottom: 1em;
  margin-left: auto;
  margin-right: auto;
}
</style>