<template>
  <div class="text-center container-fluid" :class="{'bg-dark text-light': state.dark}">
    <div class="row">
      <div class="col">
        <img alt="Vue logo" src="./assets/logo.png">
        <h3>Welcome to {{state.myName}}'s Vue.js App</h3>
        <label for="dark-mode">Dark Mode</label>
        <input id="dark-mode" type="checkbox" title="dark mode" v-model="state.dark" />
      </div>
    </div>
    <div class="row">
      <div class="col">
        <form @submit.prevent="addBot" v-if="state.addingBot">
          <p> {{state.newBotName}}</p>
          <!-- NOTE v-model is 2 way data binding -->
          <input type="text" placeholder="Name..." v-model="state.newBotName" required minlength="3">
          <button type="submit" class="btn btn-success">TO THE PIT!!</button>
          <button type="button" class="btn btn-danger" @click="state.addingBot = false">Cancel</button>
        </form>
        <button label="Add a Bot" title="Add Bot" v-else class="btn btn-info" @click="state.addingBot = true">Add A BOT!</button>
      </div>
    </div>
    <div class="row mt-5">
      <!-- NOTE v-for itterates over a collection and requires a unique 'key' that has to be a property on the object and unique to each object -->
      <!-- NOTE v-for will repeat the element it is on -->
      <div class="col-md-4 p-3 border border-dark" v-for="bot in state.enemies" :key="bot.name">
        <!-- NOTE ':' is binding of an Attribute -->
        <!-- Binding makes the string of the attribute into js -->
        <img :src="bot.imgUrl" alt="">
        <h1 :class="{
          'text-success': bot.health > 50,
          'text-warning': bot.health<=50 && bot.health > 0,
          'text-danger': bot.health == 0,
          }" class="text-bold">
          {{bot.name}} : {{bot.health}}
          </h1>
          <!-- V-if, v-else-if, v-else must be sibling elements -->
          <p v-if="bot.health === 0">GAME OVER!!!</p>
          <p v-else-if="bot.health < 50">Almost there!</p>
          <p v-else>LETS GO!</p>
        <button type="button" class="btn m-2 btn-primary" @click="clickMe(bot, 1)" :disabled="bot.health == 0">Slap</button>
        <button type="button" class="btn m-2 btn-warning" @click="clickMe(bot, 5)" :disabled="bot.health == 0">Punch</button>
        <button type="button" class="btn m-2 btn-danger" @click="clickMe(bot, 10)" :disabled="bot.health == 0">Kick</button>
        <div>
          <button type="button" class="btn m-2 btn-success" @click="bot.health = 100">Reset</button>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue'
// NOTE Every single script will need export default object
export default {
  // NOTE whatever returns from the setup is accessible in the template, and it must always be an object
  setup() {
    // NOTE this is local state only holds data that is unique to this component
    const state = reactive({
      myName: 'Mark',
      newBotName: '',
      addingBot: false,
      dark: false,
      enemies: [
        {
          name: 'J752',
          imgUrl: 'https://robohash.org/J752',
          health: 100
        },
        {
          name: 'J755',
          imgUrl: 'https://robohash.org/J755',
          health: 100
        },
        {
          name: 'R75T',
          imgUrl: 'https://robohash.org/R75T',
          health: 100
        },
        {
          name: 'V3R0N1CA',
          imgUrl: 'https://robohash.org/V3R0N1CA',
          health: 1000
        }
      ]
    })
    return {
      state,
      clickMe(bot, val) {
        bot.health -= val
        bot.health = bot.health > 0 ? bot.health : 0
      },
      addBot() {
        state.enemies.push({ name: state.newBotName, imgUrl: 'https://robohash.org/' + state.newBotName, health: 100 })
        state.newBotName = ''
      }
    }
  }
}
</script>

<style>
</style>