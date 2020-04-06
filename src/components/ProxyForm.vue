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
        If you feel you have reached this page in error, and you think that you are attempting to access a legitimate business site, contact the Help Desk at 1-800-991-3441 to report the issue with the provided information above.
      </p>
    </div>
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
