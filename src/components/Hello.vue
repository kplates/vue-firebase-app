<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- <h2>Essential Links</h2>
    <ul>
      <li>
        <a
          href="https://vuejs.org"
          target="_blank"
        >
          Core Docs
        </a>
      </li>
      <li>
        <a
          href="https://forum.vuejs.org"
          target="_blank"
        >
          Forum
        </a>
      </li>
      <li>
        <a
          href="https://chat.vuejs.org"
          target="_blank"
        >
          Community Chat
        </a>
      </li>
      <li>
        <a
          href="https://twitter.com/vuejs"
          target="_blank"
        >
          Twitter
        </a>
      </li>
      <br>
      <li>
        <a
          href="http://vuejs-templates.github.io/webpack/"
          target="_blank"
        >
          Docs for This Template
        </a>
      </li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li>
        <a
          href="http://router.vuejs.org/"
          target="_blank"
        >
          vue-router
        </a>
      </li>
      <li>
        <a
          href="http://vuex.vuejs.org/"
          target="_blank"
        >
          vuex
        </a>
      </li>
      <li>
        <a
          href="http://vue-loader.vuejs.org/"
          target="_blank"
        >
          vue-loader
        </a>
      </li>
      <li>
        <a
          href="https://github.com/vuejs/awesome-vue"
          target="_blank"
        >
          awesome-vue
        </a>
      </li>
    </ul>
     -->

    <ul>
      <li v-for="item in list">
        {{item.name}}
      </li>
    </ul>

    <div class="basic-form">
        <input type="text" v-model="name" placeholder="name"><br>
        <input type="text" v-model="desc" placeholder="desc"><br>
        <button v-on:click="addToList">Add To List</button>
    </div>

    <button v-on:click="logout">Logout</button>
  </div>
</template>

<script>
import firebase from 'firebase'
export default {
  name: 'Hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      name: '',
      desc: '',
      list: [{
        name: 'test 1234'
      }],
    }
  },
  created() {
    // alert('test');
    const userId = firebase.auth().currentUser;

    const vm = this;
    
    firebase.database().ref('notes/' + userId.uid).on('value', function(snapshot) {
      console.log(snapshot.val());  
      vm.list = snapshot.val();
    });
    //this.list = getNotes;
  },
  methods: {
    addToList: function() {
      const name = this.name;
      const desc = this.desc;
      const userId = firebase.auth().currentUser;

      firebase.database().ref('notes/' + userId.uid).push({
        name: name,
        desc: desc
      });
      console.log('adding', userId.uid, name, desc);
    },
    logout: function() {
      firebase.auth().signOut().then(() => {
        this.$router.replace('login')
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.basic-form {
  padding-bottom: 80px;
}
h1, h2 {
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
button {
  padding: 10px 20px;
  background: #42b983;
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 22px;
  outline: 0;
  cursor: pointer;
}
</style>
