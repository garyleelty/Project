<template>
  <div v-if="isLoggedIn">
    <nav>
      <ul class="nav justify-content-center">
        <li class="nav-item">
          <router-link  class="nav-link" to="/">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/musicToday">Music Today</router-link>
        </li>
        <li class="nav-item">
          <a href="#" class="nav-link" @click="signOut">Log Out</a>
        </li>
      </ul>
    </nav>
  </div>
  <router-view/>
</template>

<script>
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth';
export default {
  name: 'app',
  data(){
    return{
      email:'',
      isLoggedIn: false,
    };
  },
  mounted(){
    onAuthStateChanged(getAuth(),(user)=>{
      if(user){
        console.log('Current User: ' + user.email)
        this.email = user.email;
        this.$router.push('/wait');
        this.isLoggedIn = true;
      }else{
        this.isLoggedIn = false;
        this.$router.push('/login');
      }
    });
  },
  methods:{
    signOut(){
      const auth = getAuth();
      signOut(auth).then(() => {
        alert("Sign Out Successfully");
        this.$router.push('/login');
      }).catch((error) => {
        console.log(error);
      });
    }
  }

}
</script>

<style lang="stylus">
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
