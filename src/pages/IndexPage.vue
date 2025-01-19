<template>
  <q-page>
    <div class="player">
      <div class="wrapper">
        <div class="details">
          <div class="track-art">
            <img
              :class="`track-art image ${isPlaying ? 'rotate2' : ''}`"
              alt="STUDIO94 logo"
              src="~assets/logo.png"
              style="width: 98%; height: 98%"
            />
          </div>
          <!-- <div class="track-name">{{ nombre }}</div>
          <div class="track-artist">Track Artist</div> -->
        </div>

        <!-- <div class="slider_container">
          <i class="fa fa-volume-down"></i>
          <input
            type="range"
            min="1"
            max="100"
            value="99"
            class="volume_slider"
            onchange="setVolume()"
          />
          <i class="fa fa-volume-up"></i>
        </div> -->
        <div v-show="isPlaying" id="wave" class="loader">
          <span class="stroke"></span>
          <span class="stroke"></span>
          <span class="stroke"></span>
          <span class="stroke"></span>
          <span class="stroke"></span>
          <span class="stroke"></span>
          <span class="stroke"></span>
        </div>

        <div class="buttons text-center">
          <div class="playpause-track" @click="apagarDialog()">
            <q-icon size="2rem" name="snooze" />
          </div>
          <div v-if="!isPlaying" class="playpause-track" @click="playTrack()">
            <q-icon size="6rem" name="play_circle_outline" />
          </div>
          <div v-else class="playpause-track" @click="pauseTrack()">
            <q-icon size="6rem" name="pause_circle_outline" />
          </div>
          <div class="playpause-track" @click="volumenDialog()">
            <q-icon size="2rem" name="volume_up" />
          </div>
        </div>
      </div>
    </div>

    <q-dialog v-model="volumen" :backdrop-filter="'grayscale(100%)'">
      <q-card style="width: 300px" class="q-px-sm q-pb-md">
        <q-card-section>
          <div class="text-h6">Volúmen</div>
        </q-card-section>

        <q-item dense>
          <q-item-section avatar>
            <q-icon name="volume_up" />
          </q-item-section>
          <q-item-section>
            <q-slider
              color="#FF00AB"
              v-model="slideVol"
              :step="5"
              :min="0"
              :max="100"
              thumb-size="25px"
              label
              :label-value="slideVol + '%'"
              label-always
            />
          </q-item-section>
        </q-item>
      </q-card>
    </q-dialog>

    <q-dialog v-model="apagar" :backdrop-filter="'grayscale(100%)'">
      <q-card style="width: 300px" class="q-px-sm q-pb-md">
        <q-card-section>
          <div class="text-h6">Apagado automático</div>
        </q-card-section>
        <q-item dense>
          <q-item-section avatar>
            <q-icon name="alarm" />
          </q-item-section>
          <q-item-section>
            <q-slider
              class="mx-5"
              color="#FF0024"
              v-model="slideAlarm"
              :step="1"
              :min="0"
              :max="120"
              thumb-size="25px"
              label
              :label-value="slideAlarm + 'min'"
              label-always
            />
          </q-item-section>
        </q-item>
        <q-card-actions align="right">
          <q-btn flat label="Cancelar" color="primary" v-close-popup />
          <q-btn flat label="Iniciar" color="primary" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <div id="whatsapp">
      <a
        href="https://api.whatsapp.com/send?phone=346....302&text=Hola%20,te%20asesoramos%20por
%20whatsapp%20gestiona%20tu%20compra%20por%20este%20canal."
        target="_blank"
      >
        <svg
          width="80"
          height="80"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:svg="http://www.w3.org/2000/svg"
        >
          <!-- Created with SVG-edit - https://github.com/SVG-Edit/svgedit-->
          <g class="layer">
            <title>Layer 1</title>
            <circle
              cx="40"
              cy="40"
              fill="#fff"
              id="svg_1"
              r="38"
              stroke="#000000"
              stroke-width="0"
            />
            <path
              d="m57.81072,21.975c-4.48928,-4.5 -10.46786,-6.975 -16.82142,-6.975c-13.11429,0 -23.78571,10.67143 -23.78571,23.78571c0,4.18928 1.09286,8.28215 3.17143,11.89286l-3.375,12.32142l12.61072,-3.31072c3.47143,1.89642 7.38215,2.89286 11.36786,2.89286l0.01072,0c13.10358,0 24.01072,-10.67143 24.01072,-23.78571c0,-6.35357 -2.7,-12.32142 -7.18928,-16.82142l-0.00001,-0.00001l-0.00001,0l-0.00002,0.00001zm-16.82142,36.6c-3.55714,0 -7.03928,-0.95357 -10.07143,-2.75357l-0.71785,-0.42857l-7.47858,1.96072l1.99286,-7.29642l-0.47143,-0.75c-1.98215,-3.15 -3.02142,-6.78215 -3.02142,-10.52142c0,-10.89642 8.87143,-19.76786 19.77858,-19.76786c5.28215,0 10.24286,2.05714 13.97143,5.79642c3.72857,3.73928 6.02142,8.7 6.01072,13.98215c0,10.90714 -9.09642,19.77858 -19.99286,19.77858l0,-0.00002l-0.00001,0l-0.00001,-0.00001zm10.84286,-14.80714c-0.58928,-0.3 -3.51429,-1.73572 -4.06072,-1.92857c-0.54643,-0.20358 -0.94286,-0.3 -1.33928,0.3c-0.39642,0.6 -1.53214,1.92857 -1.88571,2.33572c-0.34286,0.39642 -0.69642,0.45 -1.28571,0.15c-3.49286,-1.74643 -5.78571,-3.11785 -8.08928,-7.07143c-0.61072,-1.05 0.61072,-0.975 1.74643,-3.24643c0.19286,-0.39642 0.09642,-0.73928 -0.05357,-1.03928c-0.15,-0.3 -1.33928,-3.225 -1.83214,-4.41429c-0.48215,-1.15714 -0.975,-0.99642 -1.33928,-1.01785c-0.34286,-0.02142 -0.73928,-0.02142 -1.13572,-0.02142c-0.39642,0 -1.03928,0.15 -1.58571,0.73928c-0.54643,0.6 -2.07858,2.03572 -2.07858,4.96072c0,2.925 2.13214,5.75357 2.42142,6.15c0.3,0.39642 4.18928,6.39642 10.15714,8.97858c3.77143,1.62857 5.25,1.76786 7.13572,1.48928c1.14643,-0.17143 3.51429,-1.43572 4.00714,-2.82857c0.49286,-1.39286 0.49286,-2.58215 0.34286,-2.82857c-0.13928,-0.26786 -0.53572,-0.41785 -1.125,-0.70714l-0.00001,-0.00001l0.00002,-0.00001l-0.00002,-0.00001z"
              fill="currentColor"
              id="svg_2"
            />
          </g>
        </svg>
      </a>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import axios from "axios";

// import puppeteer from "puppeteer";
let curr_track = document.createElement("audio");
let track_art = document.querySelector(".track-art");
export default {
  name: "IndexPage",
  data() {
    return {
      nombre: "JORGE ARCE",
      now_playing: "Sonando",
      isPlaying: false,
      volumen: false,
      apagar: false,

      slide: 1,
      lorem:
        "Lorem ipsum dolor sit amet consectetur, adipisicing elit. Natus, ratione eum minus fuga, quasi dicta facilis corporis magnam, suscipit at quo nostrum!",

      stars: 3,

      slideVol: 100,
      slideAlarm: 1,
    };
  },
  created() {
    this.loadTrack();
    // this.fetchSong();
    // this.puppeteer();
  },
  methods: {
    loadTrack() {
      console.log("reproduciendo");
      curr_track.src = "https://ssl.aloncast.com:1582/;"; // music_list[track_index].music;
      curr_track.load();

      // Or import puppeteer from 'puppeteer-core';

      // axios
      //   .get("https://ssl.aloncast.com:1582/7.html", {
      //     // headers: {
      //     //   "Access-Control-Allow-Origin": "*",
      //     // },
      //   })
      //   .then((res) => console.log(res.data))
      //   .catch((err) => console.error(err));
    },
    playTrack() {
      curr_track.play();
      this.isPlaying = true;
      setTimeout(() => {
        this.isPlaying = false;
        curr_track.pause();
      }, 5000);
      // wave.classList.add('loader');
    },
    pauseTrack() {
      curr_track.pause();
      this.isPlaying = false;
      // wave.classList.add('loader');
    },
    fetchSong() {
      fetch("http://51.89.173.53:8053/7.html", {
        headers: {
          // "Access-Control-Allow-Origin": "*",
        },
      })
        .then((response) => response.text())
        .then((data) => {
          const songInfo = this.extractSong(data);
          this.nombre = songInfo;
        })
        .catch((error) => console.error("Error:", error));
    },
    extractSong(data) {
      // Busca la parte que contiene la información de la canción
      const match = data.match(/Now On Air: (.+) - (.+)/);
      if (match && match.length > 2) {
        return `${match[1]} - ${match[2]}`;
      } else {
        return "No song information found";
      }
    },

    volumenDialog() {
      this.volumen = true;
    },
    apagarDialog() {
      this.apagar = true;
    },
  },
};
</script>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
  background: linear-gradient(to bottom, #ff000e, #ff00b3);
  font-weight: bold;
}
.player {
  display: flex;
  height: 90vh;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.wrapper {
  border: 1px solid transparent;
  padding: 10px;
  border-radius: 30px;
  /* background-color: #ddd; */
  background: #23252c00;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
}

.details {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}

.now-playing {
  font-size: 1rem;
}

.track-name {
  font-size: 2.5rem;
}

.track-artist {
  margin-top: 5px;
  font-size: 1.5rem;
}

.buttons {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.active {
  color: black;
}

.playpause-track {
  padding: 15px;
  opacity: 0.8;
  z-index: 1;
  transition: opacity 0.2s;
}

.playpause-track:hover {
  opacity: 1;
}

.slider_container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.volume_slider {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  height: 5px;
  background: #83a9ff;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
}

.volume_slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  background: white;
  border: 3px solid #3774ff;
  cursor: grab;
  border-radius: 100%;
}

.volume_slider:hover {
  opacity: 1;
}
.volume_slider {
  width: 30%;
}

.rotate2 {
  animation: rotation 8s infinite linear;
}
@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
.loader {
  margin: 2rem 0;
  height: 70px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loader .stroke {
  background: #f1f1f1;
  height: 150%;
  width: 10px;
  border-radius: 50px;
  margin: 0px 5px;
  animation: animate 1.4s linear infinite;
}
@keyframes animate {
  50% {
    height: 20%;
    background: #3c67e3;
  }

  100% {
    background: #4e00c2;
    height: 100%;
  }
}
.stroke:nth-child(1) {
  animation-delay: 0s;
}
.stroke:nth-child(2) {
  animation-delay: 0.3s;
}
.stroke:nth-child(3) {
  animation-delay: 0.6s;
}
.stroke:nth-child(4) {
  animation-delay: 0.9s;
}
.stroke:nth-child(5) {
  animation-delay: 0.6s;
}
.stroke:nth-child(6) {
  animation-delay: 0.3s;
}
.stroke:nth-child(7) {
  animation-delay: 0s;
}

:root {
  --track-art-height: 40vh;
  --track-art-width: calc(var(--track-art-height) / 1);
}

@property --rotate {
  syntax: "<angle>";
  initial-value: 132deg;
  inherits: false;
}

/* .track-art {
  margin: 25px;
  height: 250px;
  width: 250px;
  border: 2px solid #fffafa;
  background-size: cover;
  background-position: center;
  border-radius: 50%;
  -moz-box-shadow: 0px 6px 5px black;
  -webkit-box-shadow: 0px 6px 5px black;
  box-shadow: 0px 6px 5px black;
  -moz-border-radius: 190px;
  -webkit-border-radius: 190px;
  border-radius: 190px;
} */

.track-art {
  background: #191129;
  width: var(--track-art-width);
  height: var(--track-art-height);
  padding: 3px;
  position: relative;
  border-radius: 50%;
  justify-content: center;
  align-items: center;
  text-align: center;
  display: flex;
  font-size: 1.5em;
  color: rgb(88 199 250 / 0%);
  cursor: pointer;
  font-family: cursive;
}

.image {
  z-index: 2;
}

.track-art::before {
  content: "";
  width: 104%;
  height: 102%;
  border-radius: 50%;
  background-image: linear-gradient(
    var(--rotate),
    #5ddcff,
    #3c67e3 43%,
    #4e00c2
  );
  position: absolute;
  z-index: 0;
  top: -1%;
  left: -2%;
  animation: spin 2.5s linear infinite;
}
/*
.track-art::after {
  position: absolute;
  content: "";
  top: calc(var(--track-art-height) / 6);
  left: 0;
  right: 0;
  z-index: 0;
  height: 100%;
  width: 100%;
  margin: 0 auto;
  border-radius: 50%;
  transform: scale(1);
  filter: blur(calc(var(--track-art-height) / 6));
  background-image: linear-gradient(
    var(--rotate),
    #5ddcff,
    #3c67e3 43%,
    #4e00c2
  );
  opacity: 1;
  transition: opacity 0.5s;
  animation: spin 2.5s linear infinite;
} */

@keyframes spin {
  0% {
    --rotate: 0deg;
  }
  100% {
    --rotate: 360deg;
  }
}
#playervol {
  width: 350px;
  height: 50px;
  margin: 20px auto 0px auto;
}

#volume2 {
  position: absolute;
  margin: 0 auto;
  height: 5px;
  width: 300px;
  background: #555;
  border-radius: 15px;

  /* .ui-slider-range-min {
    height: 5px;
    width: 300px;
    position: absolute;
    background: #2ecc71;
    border: none;
    border-radius: 10px;
    outline: none;
  }

  .ui-slider-handle {
    width: 20px;
    height: 20px;
    border-radius: 20px;
    background: #fff;
    position: absolute;
    margin-left: -8px;
    margin-top: -8px;
    cursor: pointer;
    outline: none;
  } */
}

a {
  text-decoration: none;
}
#whatsapp {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
svg {
  width: 80px;
  display: inline-block;
  vertical-align: middle;
  filter: drop-shadow(0 1px 4px rgba(0, 0, 0, 0.4));
}
circle {
  fill: #25d366;
}
path {
  fill: #fff;
}
#whatsapp a {
  padding: 20px 0;
}
#whatsapp a::before {
  content: "Te asesoramos por whatsapp";
  display: inline-block;
  vertical-align: middle;
  padding: 5px 10px;
  margin-right: 10px;
  color: rgba(0, 0, 0, 0.8);
  background: white;
  border: 1px solid #ccc;
  border-radius: 20px;
  filter: drop-shadow(0 1px 4px rgba(0, 0, 0, 0.4));
  transition: 0.3s ease;
  opacity: 0;
}
#whatsapp:hover a::before {
  opacity: 1;
}
</style>
