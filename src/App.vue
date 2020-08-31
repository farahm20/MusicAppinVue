<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} -
          <span>{{ current.artist }}</span>
        </h2>
        <!-- {{ songs[0].src }} when it gets buld it renders a media file-->
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <!--Adding some ifelse conditions and set isplaying: false-->
          <!--if song is not playing then pause button is not shown-->
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <!--here we have created a playlist and in button there is loop that goes to every song 
         in the song array. The song is accessed by its key. 
         when we click on the song the song gets played. 
         the song that is playing is given teh class name of "song playing" 
        if the song is not playing then its given the class name of "song"-->
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >{{ song.title }} - {{ song.artist }}</button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {
        //object
        // title: 'SONG TITLE'
      },
      index: 0,
      isPlaying: false,
      songs: [
        //array of objects
        {
          title: "Beat it",
          artist: "Micheal Jackson",
          //we have to use require for the object to pull in the correct path
          src: require("./assets/Michael Jackson - Beat It (Official Video).mp3"),
        },
        {
          title: "Smooth Crimnal",
          artist: "Micheal Jackson",
          src: require("./assets/Michael Jackson - Smooth Criminal (Official Video).mp3"),
        },
      ],
      player: new Audio(), //built in
    };
  },
  methods: {
    //object of methods
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play(); //play the current song
      this.player.addEventListener("ended", function () {
        this.index++;

        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true; //means the song is playing
    },
    pause() {
      this.player.pause(); //built in function
      this.isPlaying = false;
    },
    next() {
      console.log(this.index);
      this.index++;
      console.log(this.index);
      console.log(this.songs.length);

      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      console.log(this.index);

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      console.log("prev:" + this.index);
      this.index--;
      console.log("prev:" + this.index);
      console.log("prev:" + this.songs.length);

      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      console.log("prev:" + this.index);

      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  //lifecycle method
  //as soon as the app component gets created we run this code
  created() {
    //the code plays the song
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //  this.player.play(); //will keep on playing until we add some controls
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #fff;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button :hover{
  opacity: 0.8;
}

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #cc2e5d;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #ff5858;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover {
  color: #ff5858;
}

.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, #cc2e5d, #ff5858);
}
</style>
