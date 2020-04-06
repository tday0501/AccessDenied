<template>
  <div>
    <table>
      <tr>
        <td class="table-data">URL:</td>
        <td>{{this.paramsObj.url}}</td>
      </tr>
      <tr>
        <td class="table-data">URL Category:</td>
        <td>{{this.paramsObj.cat}}</td>
      </tr>
      <tr>
        <td class="table-data">Reason:</td>
        <td>{{this.paramsObj.reason}}</td>
      </tr>
      <tr>
        <td class="table-data">Employee:</td>
        <td>{{this.paramsObj.user}}</td>
      </tr>
      <tr>
        <td class="table-data">IP Address:</td>
        <td>{{this.ip}}</td>
      </tr>
    </table>
    <div v-if="this.hasSubmitted === false ? true : false">
      <p class="message">
        If you feel you have reached this page in error, and you think that
        you are attempting to access a legitimate business site, please submit an explanation for your inquiry and
        someone
        will review your request. If you need further assistance, contact the Help Desk at
        1-800-991-3441.
      </p>
      <form v-on:submit.prevent method="POST">
        <div class="form-elements">
          <textarea rows="10" cols="70" name="reason" required v-model="justification"></textarea>
          <input type="hidden" name="url" :value="this.paramsObj.url" />
          <input type="hidden" name="url-category" :value="this.params.cat" />
          <input type="hidden" name="reason" :value="this.paramsObj.reason" />
          <input type="hidden" name="employee" :value="this.paramsObj.user" />
          <input type="hidden" name="ip-address" :value="this.ip" />
          <br />
          <br />
          <input type="submit" value="Submit" class="submit-btn" @click="submit" />
        </div>
      </form>
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
  text-align: left
}

table {
  margin-left: auto;
  margin-right: auto;
  text-align: left;
}

.table-data {
  text-align: right;
}

.submit-btn {
  height: 30px;
  width: 17em;
  border-radius: 20px;
  color: white;
  font-weight: 500;
  background-color: rgb(8, 170, 102);
  border: none;
}

.notice {
  width: 60em;
  margin: auto;
  font-weight: bold;
}
</style>
