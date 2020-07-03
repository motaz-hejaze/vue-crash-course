<template>
  <div class="hello">

    <div class="holder">

      <form @submit.prevent="addSkill">
        <validation-provider rules="min:4" v-slot="{ errors }">
          <input type="text" placeholder="Enter a skill you have" v-model="skill" name="skill">
            <transition name="alert-in" enter-active-class="animate__animated animate__flipInX" leave-active-class="animate__animated animate__flipOutX">
              <span v-if="errors.length > 0" class="alert"> {{ errors[0] }} </span>
            </transition>
        </validation-provider>
      </form>
      
      <ul>
        <transition-group name="list" enter-active-class="animate__animated animate__bounceInUp" leave-active-class="animate__animated animate__bounceOutDown">
        <li v-for="(data,index) in skills" :key='index'>
          {{ data.skill }}
          <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
        </li>
        </transition-group>
      </ul>
      <p>These are my skills</p>
    
    </div>
  </div>
</template>

<script>
import {ValidationProvider} from 'vee-validate';
import { min } from 'vee-validate/dist/rules';
import { extend } from 'vee-validate';

extend('min', {
  ...min,
  message: 'Skill Length must be 4 characters minimum'
});

export default {
  name: 'Skills',
  components: {
    ValidationProvider
  },
  data() {
    return {
      skill: '',
      skills: [
        {"skill" : "Vue.js"},
        {"skill" : "Django"}
      ]
    }
  } , 
  methods: {
    addSkill() {
        if (this.skill.length > 4) {
          this.skills.push({skill: this.skill})
          this.skill = '';
        } else {
          console.log('not valid')
        }
    },
    remove(id) {
      this.skills.splice(id,1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  scoped>
@import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css";
@import "https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

    .holder {
    background: #fff;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252;
  }

  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }

  .container {
    box-shadow: 0px 0px 40px lightgray;
  }

  input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687f7f;
  }

  .alert {
    background: #fdf2ce;
    font-weight: bold;
    display: inline-block;
    padding: 5px;
    margin-top: -20px;
  }

  .alert-in-enter-active {
    animation: bounce-in .5s;
  }

  .alert-in-leave-active {
    animation: bounce-in .5s reverse;
  }

  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }

  i {
    float:right;
    cursor: pointer;
  }
</style>
