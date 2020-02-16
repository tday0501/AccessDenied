<template>
  <div id="app">
    <ProxyPage
      v-show="this.isDLP.includes('DLP') ? false : true"
      :params="this.paramsObj"
      :ip="this.ipAddress"
    />
    <DlpPage v-show="this.isDLP.includes('DLP') ? true : false" :params="this.paramsObj" :ip="this.ipAddress"/>
  </div>
</template>

<script>
import ProxyPage from "./components/ProxyPage";
import DlpPage from "./components/DlpPage";
export default {
  components: {
    ProxyPage,
    DlpPage
  },
  data() {
    return {
      paramsObj: [],
      ipAddress: "",
      isDLP: "",
    };
  },
  method: {
  },
  created() {
    //retrieve url parameters
    const params = new URLSearchParams(window.location.search);
    const paramKeys = params.keys();
    this.paramsObj = Array.from(paramKeys).reduce(
      (acc, key) => ({ ...acc, [key]: params.get(key) }),
      {}
    )

    //check reason code to dynamically display certain page
    if (params.get("reasoncode") == null){
      this.isDLP = "Reason not given"
    } else {
      this.isDLP = params.get("reasoncode")
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
    pc.onicecandidate = (ice) => {
      if (ice && ice.candidate && ice.candidate.candidate) {
        this.ipAddress = /([0-9]{1,3}(\.[0-9]{1,3}){3}|[a-f0-9]{1,4}(:[a-f0-9]{1,4}){7})/.exec(
        ice.candidate.candidate)[1];
        pc.onicecandidate = noop;
      }
    };
  }
  
};
</script>

<style>
#app {
  font-family: Arial, Verdana, Tahoma, Helvetica;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 2em;
}
html {
  font-size: 100%;
  /* IE Hack */
  margin: 0;
  padding: 0;
  height: 100%;
}

body {
  font-size: 14px;
  background: #eef2f7;

  margin: 0;
  padding: 0;
  height: 100%;
}

img {
  border: 0;
}

table {
  border: 0;
}
</style>
