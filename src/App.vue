<template>
  <div class="text-center mt-4 container">
    <div class="row">
      <div class="col-12">
        <img alt="Vue logo" src="./assets/logo.png" />
        <form @submit.prevent="addTarget">
                <!-- NOTE Two way Databinding with a v-model -->
          <input type="text" placeholder="name" v-model="state.newTarget.name">
          <input type="text" placeholder="role" v-model="state.newTarget.role">
          <input type="number" placeholder="health" v-model="state.newTarget.health">
          <button class="btn btn-success" type="submit">Add</button>
        </form>
      </div>
      <!-- NOTE v-for itterates over a collection where the first word is the variable to represent each element (t), in the collection
    :key is a property on that object that can be used to uniquely identify it -->
      <div class="col-md-4" v-for="t in state.targets" :key="t.id">
        <h3 v-if="t.health > 1">{{ t.role }}</h3>
        <h3 v-else>Unconcious</h3>
        <h1
          :class="{ 'text-danger': t.health < 1, strikethrough: t.health < 10 }"
        >
          {{ t.name }} - {{ t.health }}
        </h1>
        <button
          :disabled="t.health < 1"
          class="btn btn-primary"
          @click="attack(1, t)"
        >
          Slap
        </button>
        <button
          :disabled="t.health < 1"
          class="btn btn-warning"
          @click="attack(2, t)"
        >
          Punch
        </button>
        <button
          :disabled="t.health < 1"
          class="btn btn-danger"
          @click="attack(3, t)"
        >
          Kick
        </button>
        <button
          v-show="t.health < 1"
          class="btn btn-info"
          @click="t.health = t.maxHealth"
        >
          defib
        </button>
      </div>
      <h1 v-if="state.allDead">All Targets Eliminated You are now the Owner of Codeworks</h1>
    </div>
  </div>
</template>

<script>  
import { reactive } from '@vue/reactivity'
import { computed } from '@vue/runtime-core'
export default {
  name: 'App',
  setup() {
    // NOTE reactive creates an object where all properties are automatically subcribed to listeners
    const state = reactive({
      targets: [{
        id: 1,
        name: 'Justin',
        role: 'Ta',
        health: 80,
        maxHealth: 80
      }, {
        id: 2,
        name: 'Mark',
        role: 'Instructor',
        health: 100,
        maxHealth: 100
      }, {
        id: 3,
        name: 'Jake',
        role: 'Boss',
        health: 120,
        maxHealth: 120
      }, {
        id: 4,
        name: 'Zach',
        role: 'SuperBoss',
        health: 125,
        maxHealth: 125
      }],
      allDead: computed(()=>{
        let dead = true;
        state.targets.forEach(t => {
          if(t.health > 1){
            dead = false;
          }
        })
        return dead
      }),
      //NOTE Two way Databinding with a form input
      newTarget: {
        name: 'Jonesy'
      }
    })
    return {
      attack(val, target) {
        console.log('slap')
        target.health -= val
        if (target.health < 0) {
          target.health = 0
        }
      },
      addTarget(){
        state.newTarget.maxHealth = state.newTarget.health
        state.targets.push(state.newTarget)
        state.newTarget = {}
      },
      state
    }
  }
}
</script>

<style scoped>
.strikethrough {
  text-decoration: line-through;
}
</style>