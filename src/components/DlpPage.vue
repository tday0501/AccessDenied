<template>
  <div data-gr-c-s-loaded="true">
    <span>&nbsp;</span>
    <div style="width:620px; margin-left:auto; margin-right:auto; text-align:center;">
      <img
        src="https://www.53.com/resources/skins/ftb/img/logos/ftb-basic.png"
        alt="Fifth Third Bank Logo"
      />
      <p style="font-size: x-large; font-weight: bold;">Unauthorized Movement of Restricted Content</p>
      <hr />
      <p style="font-size:large; font-weight:bold;">
        The Fifth Third Bank Information and Classification Handling Standard and Electronic Communication Policy restricts
        the unauthorized movement of sensitive data
        <br />
        <!--[if IE ]><br /><![endif]-->
      </p>
      <a
        href="http://intranet.info53.com/directories/policycenter/Portal/default.asp"
        alt="Go to the Fifth Third Bank Policy Center"
        target="blank"
      >
        <button>Go to the Fifth Third Bank Policy Center</button>
      </a>
      <hr />
      <table style="font-size: small; margin-left:auto; margin-right:auto; text-align:left;">
        <tbody>
          <tr>
            <td style="text-align:right;">URL:</td>
            <td>{{this.paramsObj.url}}</td>
          </tr>
          <tr>
            <td style="text-align:right;">Employee</td>
            <td>{{this.paramsObj.user}}</td>
          </tr>
          <tr>
            <td style="text-align:right;">IP Address:</td>
            <td>{{this.ip}}</td>
          </tr>
        </tbody>
      </table>
      <hr />
      <p style="font-size:small; text-align:left;">
        If you have reached this page in error and your upload of restricted data is authorized, please submit a business justification as to why you require the need to upload Restricted data. If this is an emergency, please contact the Help Desk at 1-800-991-3441 to report the issue. Please request that the ticket be submitted to the assigned group “Data Loss Prevention”.
        <br />
        <br />
      </p>
      <div v-if="this.hasSubmitted === false ? true : false">
        <form v-on:submit.prevent method="POST">
          <div class="form-elements">
            <textarea rows="10" cols="70" name="reason" required v-model="justification"></textarea>
            <input style="display: none" name="url" :value="this.paramsObj.url" />
            <input type="hidden" name="employee" :value="this.paramsObj.user" />
            <input type="hidden" name="ip-address" :value="this.ip" />
            <br />
            <br />
            <input type="submit" value="Submit" style="width: 7em;" @click="this.submit" />
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
        <b>Notice:</b> Violations of Fifth Third policies and procedures could lead to disciplinary action up to and including termination of employment. This event is currently being reviewed by the Security and Risk team to determine if further action is needed.
      </p>
    </div>
  </div>
</template>


<script>
export default {
  props: {
    params: Array,
    ip: String, //had to pass into html directly 
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
      fetch("http://5dd59d28ce4c300014403069.mockapi.io/DLP", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          name: this.paramsObj.user,
          email: this.paramsObj.url,
          ip: this.ip,
          reason: this.justification
        })
      }).then((this.hasSubmitted = !this.hasSubmitted));
    }
  }
};
</script>

<style scoped>
</style>