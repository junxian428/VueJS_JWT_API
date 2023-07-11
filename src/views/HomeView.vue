<template>
  <div class="container mt-5 text-center">
    <h3>{{ message }}</h3>

    <a href="javascript:void(0)" class="btn btn-lg btn-primary"
       @click="logout"
    >Logout</a>
  </div>
</template>

<script>
import {onMounted, ref} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";

export default {
  name: "Home",
  setup() {
    const message = ref('');
    const router = useRouter();

    /*
    onMounted(async () => {
      try {
        const {data} = await axios.get('user');

        message.value = `Hi ${data.name}`;
      } catch (e) {
        await router.push('/login');
      }
    });

    */

    
    onMounted(async () => {
      const sessionCookie = document.cookie.replace(
        /(?:(?:^|.*;\s*)access_Token\s*\=\s*([^;]*).*$)|^.*$/,
        '$1'
      );
      //console.log(sessionCookie);
      try {
        const response = await axios.get('http://localhost:8086/api/v1/user', {
          headers: {
            Authorization: `Bearer ${sessionCookie}`,
          },
        });

        const { data } = response;
        console.log(data);
        //message.value = `Hi ${data.name}`;
      } catch (error) {
        console.error(error);
        await router.push('/');
      }
    });
    

    

    const logout = async () => {
      const sessionCookie = document.cookie.replace(
        /(?:(?:^|.*;\s*)access_Token\s*\=\s*([^;]*).*$)|^.*$/,
        '$1'
      );
      console.log(sessionCookie);
      //

      axios.post('http://localhost:8086/api/v1/auth/quit')
        .then(() => {
          // Handle successful logout
          // Clear any user-related data in your Vue.js application
          // Redirect the user to the login page or any other desired route
          console.log("success");
        })
        .catch(error => {
          // Handle logout error
          console.error(error);
        });
    }

    return {
      message,
      logout
    }



  }
}
</script>