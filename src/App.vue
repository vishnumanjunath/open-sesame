<template>
  <div id="app" class="container">
    <div style="margin-top: 5rem; margin-bottom: 2rem;">
      <h1>Open Sesame</h1>
      <p>Simple and easy to Generate random password.</p>
      <KeyIcon></KeyIcon>
    </div>
    <div class="crypto_wrap">
      <h2>Customize</h2>
      <div class="flex" style="margin-top: 1rem;">
        <div class="length_slider">
          <input class="slider" name="length" type="range" min="8" max="50" v-model.number="length">
        </div>
        <div class="length_value">{{ length }}</div>
      </div>
      <div class="password_wrap" @click="copyPassword()">
        <span class="password">{{ this.password }}</span>
      </div>
      <p>What should it include?</p>
      <div class="checkboxes">
        <div class="checkbox">
          <label>ABC
            <input type="checkbox" name="uppercase" v-model="uppercase">
            <span></span>
          </label>
        </div>
        <div class="checkbox">
          <label>abc
            <input type="checkbox" name="lowercase" v-model="lowercase">
            <span></span>
          </label>
        </div>
        <div class="checkbox">
          <label>123
            <input type="checkbox" name="digits" v-model="digits">
            <span></span>
          </label>
        </div>
        <div class="checkbox" style="marign-right: 0rem 1important;">
          <label>!@#
            <input type="checkbox" name="symbols" v-model="special_char">
            <span></span>
          </label>
        </div>
      </div>
      <div>
      </div>
    </div>
  </div>
</template>
<script>
import KeyIcon from '@/components/key_icon.vue'

const getRandomValue = () => {
  return crypto.getRandomValues(new Uint32Array(1))[0] / 2 ** 32
}

const getRandomElement = (array) => {
  return array[Math.floor(getRandomValue() * array.length)]
}

const chars = {
  upper: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split(''),
  lower: 'abcdefghijklmnopqrstuvwxyz'.split(''),
  digits: '0123456789'.split(''),
  special_char: '~!@#$%^&*_-+?.:;'.split('')
}

export default {
  components: { KeyIcon },
  data () {
    return {
      length: 8,
      uppercase: false,
      lowercase: true,
      digits: false,
      special_char: false,
      password: '$JKihi45656jh76^%&WQBSJA'
    }
  },
  watch: {
    uppercase: {
      handler (val) {
        if (!this.lowercase && !this.digits && !this.special_char) {
          this.uppercase = true
        }
        this.generatePassword()
      }
    },
    lowercase: {
      handler (val) {
        if (!this.uppercase && !this.lowercase && !this.digits && !this.special_char) {
          this.lowercase = true
        }
        this.generatePassword()
      }
    },
    digits: {
      handler (val) {
        if (!this.uppercase && !this.lowercase && !this.digits && !this.special_char) {
          this.digits = true
        }
        this.generatePassword()
      }
    },
    special_char: {
      handler (val) {
        if (!this.uppercase && !this.lowercase && !this.digits && !this.special_char) {
          this.special_char = true
        }
        this.generatePassword()
      }
    },
    length: {
      handler (val) {
        this.generatePassword()
      }
    }
  },
  methods: {
    copyPassword () {
      this.$copyText(this.password)
    },

    generatePassword () {
      const some = {
        upper: this.uppercase,
        lower: this.lowercase,
        digits: this.digits,
        special_char: this.special_char
      }
      const flags = { upper: true, lower: true, digits: true, special_char: true, ...some }

      let charPool = []
      for (const key in chars) {
        if (flags[key]) {
          charPool = [...chars[key], ...charPool]
        }
      }

      this.password = Array(this.length)
        .fill(null)
        .map(() => getRandomElement(charPool))
        .join('')
    }
  }
}
</script>

<style>
.container {
  max-width: 650px !important;
  margin-left: auto;
  margin-right: auto;
  user-select:none;
  -moz-user-select:none;
  -webkit-user-select:none;
  -webkit-touch-callout:none;
  -ms-user-select:none;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

h1 {
  font-size: 3rem;
  font-weight: 300 !important;
  margin: 0rem;
}

h3 {
  font-weight: 400 !important;
}

label {
  font-weight: 500;
}

.slider {
  -webkit-appearance: none;  /* Override default CSS styles */
  appearance: none;
  width: 100%; /* Full-width */
  height: 8px; /* Specified height */
  background: #d3d3d3; /* Grey background */
  outline: none; /* Remove outline */
  opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
  -webkit-transition: .1s; /* 0.2 seconds transition on hover */
  transition: opacity .1s;
  border-radius: 4px;
}

/* The slider handle (use -webkit- (Chrome, Opera, Safari, Edge) and -moz- (Firefox) to override default look) */
.slider::-webkit-slider-thumb {
  -webkit-appearance: none; /* Override default look */
  appearance: none;
  width: 25px; /* Set a specific slider handle width */
  height: 25px; /* Slider handle height */
  border-radius: 50%;
  background: rgb(131, 198, 236); /* Green background */
  cursor: pointer; /* Cursor on hover */
}

.crypto_wrap {
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  text-align: left;
  background-color: #f8f8f8;
  padding: 0.5em 1.4em;
  line-height: 1.5em;
  border-radius: 6px;
}

.flex {
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.length_slider {
  flex: 25;
}

.length_value {
  flex: 1;
  margin-left: 20px;
  font-weight: 600;
  font-size: 1.25rem;
}

@media (min-width: 47.5rem) {
  .checkboxes > div {
    flex-basis: auto;
    margin-right: 3rem;
  }
}

.checkboxes {
  display: flex;
  flex-wrap: wrap;
  margin: 1rem 0px;
}

.checkboxes > div {
    flex-basis: auto;
    margin-right: 3rem;
}

.checkbox {
    display: inline-block;
    margin: 0px 2rem 0px 0px;
}

.checkboxs:last-child {
    margin: 0px;
}

.checkbox label {
  position: relative;
  display: inline-block;
  cursor: pointer;
  user-select: none;
  padding: 0.6em 0em 0.5em calc(1.5em + 1rem);
}

.checkbox input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0px;
    width: 0px;
}

.checkbox span {
    box-sizing: content-box;
    position: absolute;
    top: 0.5em;
    left: 0px;
    height: 1.5em;
    width: 1.5em;
    background-color: white;
    border-radius: 50%;
    border-width: 0.1rem;
    border-style: solid;
    border-color: #2c3e50;
    border-image: initial;
    transition: all 300ms ease 0s;
}

.checkbox label input:checked ~ span {
    background-color: #2c3e50;
    border-width: 0.1rem;
    border-style: solid;
    border-color: transparent;
    border-image: initial;
}

.checkbox label input:checked ~ span::after {
    display: block;
}

.checkbox label span::after {
    left: 30%;
    top: 10%;
    width: 25%;
    height: 55%;
    transform: rotate(45deg);
    border-style: solid;
    border-color: white;
    border-image: initial;
    border-width: 0px 0.2em 0.2em 0px;
    border-radius: 0.1em;
}

.checkbox span::after {
    content: "";
    position: absolute;
    display: none;
}

@media (min-width: 47.5rem) {
  .password_wrap {
      padding: 1.5rem;
      border-radius: 0px 0px 0.2rem 0.2rem;
      margin: 0px;
  }
}

.password_wrap {
  margin: 1rem 0rem 1.5rem;
  padding: 2rem;
  cursor: pointer;
  text-align: center;
  background-color: #e7e7e7dc;
  border-radius: 0.2rem;
}

@media (min-width: 47.5rem) {
  .password {
      font-size: 1.3em;
  }
}

.password {
    font-size: 1.3em;
    line-height: 1.2;
    position: relative;
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    -webkit-box-pack: center;
    justify-content: center;
    text-align: center;
    overflow-wrap: break-word;
    word-break: break-word;
    padding: 0px 1rem;
    transition: all 300ms ease 0s;
}

.password_wrap:hover {
  background-color: #e0e0e0dc;
}

.password_wrap::before {
    content: "Copied!";
    display: flex;
    color: rgb(240, 240, 240);
    font-weight: bold;
    -webkit-box-align: center;
    align-items: center;
    -webkit-box-pack: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0px;
    left: 0px;
    pointer-events: none;
    opacity: 0;
    transition: opacity 300ms ease 0s;
}
</style>
