<template>
  <div id="app">
    <header>
      <h1>MiniPlayer</h1>
    </header>

    <main>
      <section class="player">
        <h3 class="song-title">
          {{ current.title }} -
          <span>{{ current.artist }}</span>
        </h3>
        <div class="controls">
          <button class="previous" v-on:click="prev">
            <i class="fas fa-backward"></i>
          </button>
          <button class="play" v-if="!isPlaying" v-on:click="play">
            <i class="fas fa-play-circle"></i>
          </button>
          <button class="pause" v-else v-on:click="pause">
            <i class="fas fa-pause-circle"></i>
          </button>
          <button class="next" v-on:click="next">
            <i class="fas fa-forward"></i>
          </button>
        </div>
      </section>
      <hr />
      <section class="playlist">
        <h3>My Playlist</h3>
        <button
          v-for="song in songs"
          v-on:click="play(song)"
          :key="song.src"
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >{{song.title}} - {{song.artist}}</button>
      </section>
    </main>
  </div>
</template>

<!--Scripts-->
<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Tic Toc",
          artist: "Christian Lalama",
          src: require("./assets/Christian Lalama-Tic Toc.mp3"),
        },
        {
          title: "Middle",
          artist: "Dj Snake",
          src: require("./assets/DJ Snake-Middle.mp3"),
        },
        {
          title: "I Took A Pill",
          artist: "Mike Posner",
          src: require("./assets/MikePosner-I took a Pill.mp3"),
        },
        {
          title: "I Feel It Coming",
          artist: "The Weeknd",
          src: require("./assets/The Weeknd-I Feel It Coming.mp3"),
        },
        {
          title: "The Hills",
          artist: "The Weeknd",
          src: require("./assets/The Weeknd-The Hills.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<!--Styling-->
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

body {
  background: #f2f2f2;
  display: grid;
  place-items: center;
  height: 100vh;
}

#app {
  margin: 0 auto;
  box-shadow: 0 0 25px #cccccc;
  width: 350px;
}

header {
  display: grid;
  place-items: center;
  background: blueviolet;
  color: #f2f2f2;
  padding: 10px 0;
}

main {
  margin: 0 auto;
  width: 100%;
  max-width: 500px;
  padding: 25px;
}

.song-img {
  display: grid;
  place-items: center;
  margin: 0 auto;
  width: 250px;
  height: 250px;
}

.song-title {
  margin-top: 20px;
  font-size: 1.5em;
  text-align: center;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 15px;
}

.play,
.pause {
  font-size: 3.5em;
  border: none;
  color: blueviolet;
  cursor: pointer;
}

.play:hover,
.pause:hover {
  opacity: 0.7;
}

.previous,
.next {
  padding: 5px 0;
  font-size: 1.5em;
  color: blueviolet;
  cursor: pointer;
  transition: all 300ms ease-out;
}

.previous {
  padding-right: 20px;
}
.previous:hover {
  transform: translateX(-5px);
}

.next {
  padding-left: 20px;
}
.next:hover {
  transform: translateX(5px);
}

.playlist h3 {
  text-align: center;
  margin: 15px 0;
  font-size: 1.4em;
  color: blueviolet;
}

button {
  background: none;
  border: none;
  outline: none;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.playlist .song {
  display: block;
  width: 100%;
  padding: 5px;
  font-size: 1em;
  cursor: pointer;
}

.playlist .song:hover {
  color: blueviolet;
}

.playlist .song.playing {
  background: blueviolet;
  color: #f2f2f2;
  font-weight: 600;
}

@media screen and (min-width: 1024px) {
  #app {
    width: 500px;
  }
  main {
    display: flex;
    padding: 0px;
  }

  .player,
  .playlist {
    margin: 15px;
    width: 250px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .song-img {
    width: 200px;
    height: 200px;
  }

  hr {
    margin: 15px 0;
  }
}
</style>
