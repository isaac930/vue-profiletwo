<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input type= "text" v-model="search" class="input">
        <p>search term - {{search}}</p>
        <div v-for="profile in matchedProfile" :key="profile.name">{{profile.name}}</div>
      </div>
      <div class="buttons">
        <button @click="sortAsc">▲</button>
        <button @click="sortDesc">▼</button>
      </div>

      <ProfileCard
        v-for="profile in profiles"
        :key="profile.id"
        :profile="profile"
        class="profile"
      />

      <div class="icons-note">
        Icons made by
        <a href="https://www.flaticon.com/authors/freepik" title="Freepik">Freepik</a> from
        <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
      </div>
    </div>

    <div class="section">
      <p class="header">Add new profile:</p>
      <form @submit="addProfile">
      <div class="flex-row">
        <label class="label">Name:</label>
        <input required type="text" v-model="name" placeholder="enter name" class="form-control">
      </div>
      <div class="flex-row">
        <label class="label" for="filter">Email:</label>
        <input required type="email" v-model="email" placeholder="enter email" class="form-control">
      </div>
      <div class="flex-row">
        <label class="label">Specialisation:</label>
        <input required type="text" v-model="description" placeholder="enter description" class="form-control">
      </div>
      <button>Add</button>
    </form>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";
import Profiles from "./components/Profiles";
import {computed,ref,watch} from 'vue';

export default {
  name: "App",

  components: {
    ProfileCard,
    Profiles
  },

  setup(){
const search = ref('')
const profiles = ref([
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53
        }
      ])

      watch(search, () =>{
        const matchedProfile = computed(()=>{
  
  return profiles.value.filter((name) => name.includes(search.value))
})
return {search,matchedProfile}
      })
  }
,
  data() {
    return {
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53
        }
      ]
    };
  },

  methods: {
    sortAsc() {
      this.profiles.sort(function(a, b) {
        return a.likes - b.likes;
      });
    },

    sortDesc() {
      this.profiles.sort(function(a, b) {
        return b.likes - a.likes;
      });
    },

    //lets add a method to add new profiles
    addProfile(e){
      e.preventDefault()
      const newProfile = {
        id:Date.now(),
        name: this.name,
        description: this.description,
        likes: 0
      }
      // Send up to parent
      this.$emit('add-profile', newProfile);

      this.name = '';
      this.email = '';
      this.description = '';
      this.likes = 0;
    },
    //delete profile
    deleteProfile(id){
      this.profiles = this.profiles.filter((profile) => profile.id !== id)
    }
  }
};
</script>

<style>
#app {
  font-family: "Roboto", helvetica, arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: relative;

  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );

  font-size: 1.5em;
}

button {
  display: block;
  padding: 1em;
  width: 100%;
  background-color: #41B883;
  border: 1px solid;
  color: #fff;
  cursor: pointer;
  font-size: 0.75em;
  font-weight: 600;
  text-shadow: 0 1px 0 rgba(black, 0.2);
}

.content {
  display: flex;
}

.section {
  width: 100%;
  min-width: 300px;
  padding: 2em;
  margin-top: 30px;
  position: relative;
  background: rgba(0, 0, 0, 0.15);
}

@media screen and (min-width: 600px) {
  .section {
    width: 50vw;
    max-width: 15em;
  }
}

.header {
  color: #fff;
}

.section::before {
  content: "";
  position: absolute;
  top: -2px;
  left: 0;
  height: 2px;
  width: 100%;
  background: #35c3c1;
}

.flex-row {
  display: flex;
  margin-bottom: 1em;
}

.label {
  width: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;

  background: #f5f6f8;
}

.input {
  flex: 1;
  padding: 1em;
  border: 0;
  color: #8f8f8f;
  font-size: 1rem;
}

.buttons {
  display: flex;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22);
  margin-top: 30px;
}

.profile {
  margin-top: 20px;
}

.icons-note {
  margin-top: 30px;
  font-size: 10px;
}
</style>
