<script>
import { ref } from 'vue'
///
import { CognitoUserPool, CognitoUserAttribute, CognitoUser, AuthenticationDetails} from 'amazon-cognito-identity-js';

const poolData = {
  UserPoolId: 'us-east-1_LDTH1c0wj',
  ClientId: '1gtloqe56inp0fkj4mu4p1msq5'
};
const userPool = new CognitoUserPool(poolData);

export default {
  data() {
    return {
      username: 'kaihuang',
      password: 'KaiHuang123!',
      email: 'sgkaihuang@gmail.com',
      phoneNumber: '+15555555555',
      authCode: '574149'
    }
  },

  methods: {
    signUp() {
      const attributeList = [
        new CognitoUserAttribute({
          Name: 'email',
          Value: this.email
        }),
        new CognitoUserAttribute({
          Name: 'phone_number',
          Value: this.phoneNumber
        })
      ];
      
      // username, password are needed, attributes are in a list
      userPool.signUp(this.username, this.password, attributeList, null, (err, result) => {
        if (err) {
          console.log(err);
          return;
        }
        console.log(result);
      });
    },

    confirmSignUp() {
      const cognitoUser = new CognitoUser({
        Username: this.username,
        Pool: userPool
      });
      
      cognitoUser.confirmRegistration(this.authCode, true, (err, result) => {
        if (err) {
          console.log(err);
          return;
        }
        console.log(result);
      });
    },

    signIn() {
      const authenticationData = {
        Username: this.username,
        Password: this.password
      };
      const authenticationDetails = new AuthenticationDetails(authenticationData);
      
      const cognitoUser = new CognitoUser({
        Username: this.username,
        Pool: userPool
      });
      
      cognitoUser.authenticateUser(authenticationDetails, {
        onSuccess: function(result) {
          console.log(result);
        },
        onFailure: function(err) {
          console.log(err);
        }
      });
    },

    greet() {
    // `this` inside methods points to the current active instance
    console.log("hello")
  }
  }
}
///

const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <button v-on:click="signUp">Add 2</button>
  <button v-on:click="confirmSignUp">confirmSignUp</button>
  <button v-on:click="signIn">SignIn</button>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter. The badass wolf.
  </p>
  <p>
    Install
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
