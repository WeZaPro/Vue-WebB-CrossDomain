<template>
  <div id="app">
    <h1>WEB Site B -HomePage -Liff</h1>
    <img :src="profile.pictureUrl" width="60" height="60" />
    <!-- {{ profile.displayName }} -->
    <p>display Name : {{ profile.displayName }}</p>
    <p>userID : {{ profile.userId }}</p>
    <p>param : {{ this.getParam }}</p>
  </div>
  <div id="btn">
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
  mounted() {
    //(((((())))))
    let uri = window.location.search.substring(1);
    let params = new URLSearchParams(uri);
    liff
      .init({ liffId: "1656824759-lQKpOazZ" })
      .then(() => {
        if (!liff.isLoggedIn()) {
          liff.login();
        } else {
          this.loggedIn = liff.isLoggedIn();
          alert(`Line Login--> `);
          console.log("isLoggedIn--> ", liff.isLoggedIn());
          console.log("getIDToken--> ", liff.getIDToken());
          console.log("getContext--> ", liff.getContext());
          console.log("getOS--> ", liff.getOS());
          console.log("isInClient--> ", liff.isInClient());
          //console.log("getAdvertisingId--> ", liff.getAdvertisingId());
          //console.log("getAId--> ", liff.getAId());
          console.log("getVersion--> ", liff.getVersion());
          //console.log("getEnvironment--> ", liff.getEnvironment());

          // get queryString
          this.getParam = params.get("param");
          console.log("param--->", this.getParam);

          this.getProfile();
          // this.getEnvironment();
          this.getFriendship();
        }
      })
      .catch((err) => {
        this.occoredError = "error:" + err;
      });
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
      // console.log("liff.getContext().type--> ", liff.getContext().type);
      // if (liff.getContext().type !== "none") {
      //   await liff.sendMessages([
      //     {
      //       type: "sticker",
      //       stickerId: 1,
      //       packageId: 1,
      //     },
      //   ]);
      //   alert("Message sent");
      // }

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
            // {
            //   type: "text",
            //   text: "Register",
            // },
            // {
            //   type: "text",
            //   text: profile.displayName,
            // },
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

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#btn {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
