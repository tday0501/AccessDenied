<template>
  <div id="app">
    <div class="main">
      <div class="logo-container">
        <img class="logo" src="./assets/FT-Logo.svg" alt="Fifth Third Bank Logo" />
      </div>
      <div class="header-container">
        <p class="header">{{this.header_text}}</p>
      </div>
      <p class="subheading">{{this.subheading_text}}</p>
      <v-btn
        rounded
        dense
        color="rgb(8, 170, 102)"
        dark
        href="http://intranet.info53.com/directories/policycenter/Portal/default.asp"
        alt="Bank Policy Center"
        target="blank"
        width="15em"
      >Bank Policy Center</v-btn>
    </div>
    <div class="form">
      <proxy-form
        v-show="this.page === 'Proxy' ? true : false"
        :params="this.paramsObj"
        :ip="this.ipAddress"
      />
      <dlp-form
        v-show="this.page === 'DLP' ? true : false"
        :params="this.paramsObj"
        :ip="this.ipAddress"
      />
      <file-form v-show="this.page === 'File'  ? true : false" />
    </div>
  </div>
</template>

<script>
import ProxyForm from "./components/ProxyForm";
import DlpForm from "./components/DlpForm";
import FileForm from "./components/FileForm";

export default {
  components: {
    ProxyForm,
    DlpForm,
    FileForm
  },
  data() {
    return {
      paramsObj: [],
      ipAddress: "",
      page: "",
      header_text: "ATTENTION: Access Denied",
      subheading_text:
        "The Fifth Third Bank Electronic Communications policy and Employee Policy Manual restrict access to this web page"
    };
  },
  method: {},
  created() {
    //retrieve url parameters
    const params = new URLSearchParams(window.location.search);
    const paramKeys = params.keys();
    this.paramsObj = Array.from(paramKeys).reduce(
      (acc, key) => ({ ...acc, [key]: params.get(key) }),
      {}
    );

    //check reason code to dynamically display certain page
    let reasonCode = params.get("reasoncode");
    let category = params.get("cat");
    if (category.includes("File host") || category.includes("Web search")) {
      this.page = "File";
    } else if (reasonCode.includes("DLP")) {
      this.page = "DLP";
      (this.header_text = "Unauthorized Movement of Restricted Content"),
        (this.subheading_text =
          "The Fifth Third Bank Information and Classification Handling Standard and Electronic Communication Policy restricts" +
          "the unauthorized movement of sensitive data");
    } else {
      this.page = "Proxy";
    }

    //get IP
    window.RTCPeerConnection =
      window.RTCPeerConnection ||
      window.mozRTCPeerConnection ||
      window.webkitRTCPeerConnection; //compatibility for Firefox and chrome
    var pc = new RTCPeerConnection({ iceServers: [] }),
      noop = function() {};
    pc.createDataChannel(""); //create a bogus data channel
    pc.createOffer(pc.setLocalDescription.bind(pc), noop); // create offer and set local description
    pc.onicecandidate = ice => {
      if (ice && ice.candidate && ice.candidate.candidate) {
        this.ipAddress = /([0-9]{1,3}(\.[0-9]{1,3}){3}|[a-f0-9]{1,4}(:[a-f0-9]{1,4}){7})/.exec(
          ice.candidate.candidate
        )[1];
        pc.onicecandidate = noop;
      }
    };
  }
};
</script>

<style>
#app {
  font-family: Sans-serif;
  text-align: center;
}

html {
  font-size: 100%;
  /* IE Hack */
  margin: 0;
  padding: 0;
  height: 100vh;
}

body {
  font-size: 14px;
  background: rgb(255, 255, 255);
  margin: 0;
  padding: 0;
  height: 100vh;
}

.main {
  margin-bottom: 2em;
}

.logo-container {
  display: flex;
  justify-content: center;
  align-content: center;
  height: 6.1em;
}

.logo {
  width: 110px;
  height: auto;
}

.header-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 5em;
  width: 100%;
  background-color: rgb(28, 39, 88);
  color: rgb(255, 255, 255);
}

.header {
  font-size: x-large;
  font-weight: bold;

}

.subheading {
  font-size: large;
  font-weight: bold;
  width: 30em;
  margin: 1.5em auto;
}

a {
  display: flex;
  justify-content: center;
  align-content: center;
  text-decoration: none;
}

</style>
