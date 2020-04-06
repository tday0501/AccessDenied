<template>
  <div data-gr-c-s-loaded="true">
    <div class="body-container">
      <table>
        <tbody>
          <tr>
            <td class="table-data">URL:</td>
            <td>{{this.paramsObj.url}}</td>
          </tr>
          <tr>
            <td class="table-data">Employee</td>
            <td>{{this.paramsObj.user}}</td>
          </tr>
          <tr>
            <td class="table-data">IP Address:</td>
            <td>{{this.ip}}</td>
          </tr>
        </tbody>
      </table>
      <p class="message">
        If you have reached this page in error and your upload of restricted data is authorized, please submit a vendor, client contact and a business justification as to why you require the need to upload Restricted data. If this is an emergency, please contact the Help Desk at 1-800-991-3441 to report the issue. Please request that the ticket be submitted to the assigned group “Data Loss Prevention”.
        <br />
        <br />
      </p>
      <div v-if="this.hasSubmitted === false ? true : false">
        <form v-on:submit.prevent method="POST">
          <div class="form-elements">
            <textarea
              class="client-text"
              name="reason"
              required
              v-model="vendor"
              placeholder="Please provide a vendor"
            ></textarea>
            <textarea
              class="justify-text"
              name="reason"
              required
              v-model="justification"
              placeholder="Please provide a business justification"
            ></textarea>
            <input type="hidden" name="url" :value="this.paramsObj.url" />
            <input type="hidden" name="employee" :value="this.paramsObj.user" />
            <input type="hidden" name="ip-address" :value="this.ip" />
            <br />
            <br />
            <input type="submit" value="Submit" class="submit-btn" @click="this.submit" />
          </div>
        </form>
      </div>
      <div v-if="this.hasSubmitted === true ? true : false">
        <!-- You can customize the thankyou message by editing the code below -->
        <h2>
          <em>Thanks</em> for contacting us! Check back here soon!
        </h2>
      </div>
      <p>
        <br />If you believe your job responsibilities require the sending of this information, please consult your manager on appropriateness and refer to the DLP Intranet Landing Page (ILP) for instructions on how to request the process to do so.
        <br />
        <br />For a list of frequently asked questions regarding this process, please refer to the
        <a
          href="”https://inside.info53.com/ep/myec/!ut/p/a1/tVTLbsIwEPyVXJDaQ-Q1dl7HIBAEFUKBCJILMonTus0LcFHp1zekVXspL8ndm1cz45nValGEligq2F48MSnKgmXHd2SuMGB3QCk8QL_dBXcwmrpk3AcaWDUgrAFD18Yzj7bBfiQd8CZ45M9t7PkT6zo--CQYdDv-sAcBJuAYBL7rEn-BIhTFhazkMwpFkZbbvPGu7Xj8thXy0IK_uprIqhakIuNazNai4LIFCZNMy8rdTqu2fM-LhnCXZNX98Y8qFgkK2xTWjoFTHcfAdYqpqTsxMN0GwhlPHGKRn0wnyr0u04WpNoBzY_sCnPYQ1iatUy48k6DZjanPC45AtaChWLCnXJCqjmypdmgqFgxUrw2-fYbDK06OeNlsIrc-HGUh-btEy3-7HFUeBLlNDvrrtHZpVPuPeZrnq_FYZ-vfxsL9BNPKDws!/dl5/d5/L2dBISEvZ0FBIS9nQSEh/”"
        >Data Loss Prevention ILP</a>.
        <br />
        <br />
        Notice: Violations of Fifth Third policies and procedures could lead to disciplinary action up to and including termination of employment. This event is currently being reviewed by the Security and Risk team to determine if further action is needed.
      </p>
    </div>
  </div>
</template>


<script>
//import axios from "axios";

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
.body-container {
  width: 620px;
  margin: auto;
  text-align: center;
}

table {
  font-size: small;
  margin-left: auto;
  margin-right: auto;
  text-align: left;
}

.table-data {
  text-align: right;
}

.client-text {
  margin-bottom: 0.5em;
  height: 1.1em;
  line-height: 1.1em;
  padding: 5px;
  width: 40em;
}

.justify-text {
  height: 10em;
  line-height: 1.1em;
  padding: 5px;
  width: 40em;
}

.message {
  font-size: small;
  text-align: left;
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
  text-align: center;
  font-weight: bold;
}
</style>