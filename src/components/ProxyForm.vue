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
      <p
        class="message"
      >  If you feel you have reached this page in error, and you think that
        you are attempting to access a legitimate business site, please submit an explanation for your inquiry and
        someone
        will review your request. If you need further assistance, contact the Help Desk at
        1-800-991-3441.</p>
      <form v-on:submit.prevent method="POST">
        <div class="form-elements">
          <v-row class="center-textarea">
            <v-col md="4" style="padding: 0;">
              <v-textarea
                outlined
                filled
                name="reason"
                required
                full-width="false"
                v-model="justification"
                label="Please provide a business justification"
              ></v-textarea>
            </v-col>
          </v-row>
          <input type="hidden" name="url" :value="this.paramsObj.url" />
          <input type="hidden" name="url-category" :value="this.params.cat" />
          <input type="hidden" name="reason" :value="this.paramsObj.reason" />
          <input type="hidden" name="employee" :value="this.paramsObj.user" />
          <input type="hidden" name="ip-address" :value="this.ip" />
          <v-btn
            rounded
            dense
            color="rgb(8, 170, 102)"
            dark
            type="submit"
            value="Submit"
            width="15em"
            @click="submit"
          >Sumbit</v-btn>
        </div>
      </form>
    </div>
     <div v-if="this.hasSubmitted === true ? true : false" class="goodbye">
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
      fetch("http://slgramihqaims90.info53.com:3333/api/v1/email/send", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          recipients: "tran.day@53.com",
          subject: "Proxy Request",
          body:
            "<html><ul style='list-style: none'><li>Username: " +
            this.paramsObj.user +
            "</li><li>Url: " +
            this.paramsObj.url +
            "</li><li>URL Category: " +
            this.paramsObj.cat +
            "</li><li>IP Address: " +
            this.ip +
            "</li><li>Reason: " +
            this.justification +
            "</li></ul></html>"
        }),
        redirect: "follow",
        mode: "no-cors"
      })
        .then(response => response.text())
        .then(result => console.log(result))
        .catch(error => console.log("error", error));
      this.hasSubmitted = !this.hasSubmitted;
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
