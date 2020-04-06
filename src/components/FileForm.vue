<template>
  <div>
    <div>
      <p class="message">
        If you feel you have reached this page in error, and you think that
        you are attempting to access a legitimate business site, please follow the link below to submit a
        request. If you need further assistance, contact the Help Desk at
        1-800-991-3441.
        <br />
        <br />
      </p>
      <v-btn
        rounded
        dense
        color="rgb(8, 170, 102)"
        dark
        href="https://thebank.info53.com/teams/InfoSec/InfoSecTeam/AS/_layouts/15/FormServer.aspx?XsnLocation=https://thebank.info53.com/teams/InfoSec/InfoSecTeam/AS/Proxy%20Entitlements/Forms/template.xsn&SaveLocation=https%3A%2F%2Fthebank%2Einfo53%2Ecom%2Fteams%2FInfoSec%2FInfoSecTeam%2FAS%2FProxy%20Entitlements&ClientInstalled=true&DefaultItemOpen=1&Source=https%3A%2F%2Fthebank.info53.com%2Fteams%2FInfoSec%2FInfoSecTeam%2FAS%2FSitePages%2FEscalated_Proxy_Requests.aspx"
        alt="Bank Policy Center"
        target="blank"
        width="15em"
      >Request Access</v-btn>
    </div>
    <div v-if="this.hasSubmitted === true ? true : false">
      <!-- You can customize the thankyou message by editing the code below -->
      <h2>
        <em>Thanks</em> for contacting us! Check back here soon!
      </h2>
    </div>
    <br />
    <br />
    
    
    <div class="notice">
      Notice: Due to the continued current and emerging threat
      of email based attacks on the Fifth Third network, we have determined that access to web based email
      services will be prohibited indefinitely in accordance with the Electronic Communications Policy.
    </div>
  </div>
</template>

<script>
export default {
  props: {
    params: Object,
    ip: String //had to pass into html directly
  },
  data() {
    return {
      paramsObj: this.params,
      justification: "",
      hasSubmitted: false
    };
  },
  methods: {
    submit() {
      fetch("https://5dd59d28ce4c300014403069.mockapi.io/Proxy", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          url: this.paramsObj.url,
          urlCat: this.paramsObj.urlCat,
          reason: this.paramsObj.reason,
          user: this.paramsObj.user,
          explanation: this.justification,
          ip: this.ip
        })
      }).then((this.hasSubmitted = !this.hasSubmitted));
    }
  }
};
</script>

<style scoped>
.message {
  width: 60em;
  margin: 2em auto;
  text-align: left;
}

table {
  margin-left: auto;
  margin-right: auto;
  text-align: left;
}

.table-data {
  text-align: right;
  font-weight: 600;
  padding-right: 5px;
}

.center-textarea {
  display: flex; 
  justify-content: center;
}

.notice {
  width: 60em;
  margin: auto;
  font-weight: bold;
}

.goodbye {
  margin-top: 3em;
}
</style>
