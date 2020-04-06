<template>
  <div>
    <table>
      <tr>
        <td class="table-data">URL:</td>
        <td>{{this.paramsObj.url}}</td>
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
      >If you have reached this page in error and your upload of restricted data is authorized, please submit a vendor, client contact and a business justification as to why you require the need to upload Restricted data. If this is an emergency, please contact the Help Desk at 1-800-991-3441 to report the issue. Please request that the ticket be submitted to the assigned group “Data Loss Prevention”.</p>
      <v-form v-on:submit.prevent method="POST">
        <v-row class="center-textarea">
          <v-col md="4" style="padding: 0;">
            <v-text-field
              outlined
              filled
              name="reason"
              required
              full-width="false"
              v-model="vendor"
              label="Please provide a vendor"
            ></v-text-field>
          </v-col>
        </v-row>
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
      </v-form>
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
      <p>
        <br />If you believe your job responsibilities require the sending of this information, please consult your manager on appropriateness and refer to the DLP Intranet Landing Page (ILP) for instructions on how to request the process to do so.
        <br />
        <br />For a list of frequently asked questions regarding this process, please refer to the
        <a
          href="”https://inside.info53.com/ep/myec/!ut/p/a1/tVTLbsIwEPyVXJDaQ-Q1dl7HIBAEFUKBCJILMonTus0LcFHp1zekVXspL8ndm1cz45nValGEligq2F48MSnKgmXHd2SuMGB3QCk8QL_dBXcwmrpk3AcaWDUgrAFD18Yzj7bBfiQd8CZ45M9t7PkT6zo--CQYdDv-sAcBJuAYBL7rEn-BIhTFhazkMwpFkZbbvPGu7Xj8thXy0IK_uprIqhakIuNazNai4LIFCZNMy8rdTqu2fM-LhnCXZNX98Y8qFgkK2xTWjoFTHcfAdYqpqTsxMN0GwhlPHGKRn0wnyr0u04WpNoBzY_sCnPYQ1iatUy48k6DZjanPC45AtaChWLCnXJCqjmypdmgqFgxUrw2-fYbDK06OeNlsIrc-HGUh-btEy3-7HFUeBLlNDvrrtHZpVPuPeZrnq_FYZ-vfxsL9BNPKDws!/dl5/d5/L2dBISEvZ0FBIS9nQSEh/”"
        >Data Loss Prevention ILP.</a>
        <br />Notice: Violations of Fifth Third policies and procedures could lead to disciplinary action up to and including termination of employment. This event is currently being reviewed by the Security and Risk team to determine if further action is needed.
      </p>
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
      vendor: "",
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
          subject: "DLP Request",
          body:
            "<html><ul style='list-style: none'><li>Username: " +
            this.paramsObj.user +
            "</li><li>Url: " +
            this.paramsObj.url +
            "</li><li>IP Address: " +
            this.ip +
            "</li><li>Vendor: " +
            this.vendor +
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
  padding-bottom: 3em;
  font-weight: bold;
}

.goodbye {
  margin-top: 3em;
}
</style>
