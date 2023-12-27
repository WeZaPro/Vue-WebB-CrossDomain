<script setup></script>

<template>
  <div id="app">
    <!-- <nav>
      <ul>
        <li>
          <router-link to="/" class="text-white decor-none">Home</router-link>
        </li>
        <li>
          <router-link to="/about" class="text-white decor-none"
            >About</router-link
          >
        </li>
      </ul>
    </nav> -->
    <h1>WEB SECOND PAGE App Page Liff App</h1>
    <!-- <router-view></router-view> -->
  </div>
  <div id="liff">
    <img :src="profile.pictureUrl" width="60" height="60" />
    <!-- {{ profile.displayName }} -->
    <p>display Name : {{ profile.displayName }}</p>
    <p>userID : {{ profile.userId }}</p>
    <p>param : {{ this.getParam }}</p>

    <button @click="openLineChat">Line@</button>
    <button @click="sendMsg">chat</button>
  </div>
</template>

<script>
//import HelloWorld from "./components/HelloWorld.vue";
import liff from "@line/liff";

export default {
  name: "App",
  components: {
    // HelloWorld,
  },
  data() {
    return {
      loggedIn: false,
      profile: {},
      displayName: "",
      userId: "",
      alert: "",
      getParam: "",
    };
  },
  async mounted() {
    await liff.init({ liffId: "1656824759-lQKpOazZ" });
    // const profile = await liff.getProfile();

    //alert("start");
    //(((((())))))
    // let uri = window.location.search.substring(1);
    // let params = new URLSearchParams(uri);
    // liff
    //   .init({ liffId: "1656824759-lQKpOazZ" })
    //   .then(() => {
    //     if (!liff.isLoggedIn()) {
    //       liff.login();
    //     } else {
    //       this.loggedIn = liff.isLoggedIn();
    //       alert(`Line Login--> `);

    //       // get queryString
    //       this.getParam = params.get("param");
    //       console.log("param--->", this.getParam);

    //       this.getProfile();
    //       // this.getEnvironment();
    //       this.getFriendship();
    //     }
    //   })
    //   .catch((err) => {
    //     this.occoredError = "error:" + err;
    //   });
  },
  methods: {
    async getProfile() {
      await liff.getProfile().then((profile) => {
        console.log("profile--> ", profile);
        this.profile = profile;
        // console.log("this.profile--> ", this.profile);
      });
    },
    async getFriendship() {
      await liff.getFriendship().then((data) => {
        console.log("getFriendship--> ", data);
        if (data.friendFlag) {
          console.log("getFriendship--> = true");
          this.alert = data.friendFlag;
          alert(`data.friendFlag--> ${this.alert}`);
        }
      });
    },
    async getAccessToken() {
      console.log("token--> ");
      await liff.getAccessToken().then((token) => {
        console.log("token--> ", token);
      });
    },

    openLineChat() {
      console.log("openLineChat--> ");
      window.open("https://line.me/ti/p/@889mtekm", "_blank");
    },

    async sendMsg() {
      const profile = await liff.getProfile();
      console.log("userId---> " + profile.userId);

      if (!liff.isInClient()) {
        window.alert(
          "This button is unavailable as LIFF is currently being opened in an external browser."
        );
      } else {
        await liff
          .sendMessages([
            {
              type: "text",
              text: `Register/${profile.displayName}`,
            },
          ])
          .then(() => {
            window.alert(`Message sent Register ` + profile.userId);
          })
          .catch((error) => {
            window.alert("Error sending message: " + error);
          });
      }
    },
  },
};
</script>

<style scoped>
nav ul {
  display: flex;
  list-style: none;
  gap: 10px;
}

nav ul li {
  background-color: hsl(252, 48%, 57%);
  padding: 4px 6px;
  border-radius: 5px;
  color: #fff;
  transition: 0.5s;
}

nav ul li:hover {
  background-color: hsl(252, 38%, 38%);
}

.text-white {
  color: white;
}

.decor-none {
  text-decoration: none;
}
</style>
