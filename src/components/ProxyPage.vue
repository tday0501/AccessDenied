<template>
  <div>
      <span>&nbsp;</span>
    <div style="width:620px; margin-left:auto; margin-right:auto; text-align:center;">
        <img src='https://www.53.com/resources/skins/ftb/img/logos/ftb-basic.png' alt='Fifth Third Bank Logo' />
        <p style='font-size: x-large; font-weight: bold;'>ATTENTION: Access Denied</p>
        <hr />
        <p style='font-size:large; font-weight:bold;'>The Fifth Third Bank Electronic Communications policy
            and<br />Employee Policy Manual restrict access to this web page<br />  </p>
            <!--[if IE ]><br /><![endif]-->
            <form id='button' action='http://intranet.info53.com/directories/policycenter/Portal/default.asp'
                method='get'><input type='submit' value='Go to the Fifth Third Bank Policy Center'
                    formtarget='_blank' /></form>
      
        <hr />
        <ul style="list-style-type:disc">
            <a id="redirectcode" name="redirectcode"></a>
        </ul>
        <table style='font-size: small; margin-left:auto; margin-right:auto; text-align:left;'>
            <tr>
                <td style='text-align:right;'>URL:</TD>
                <td>{{this.paramsObj.url}}</td>
            </tr>
            <tr>
                <td style='text-align:right;'>URL Category:</TD>
                <td>{{this.paramsObj.cat}}</td>
            </tr>
            <tr>
                <td style='text-align:right;'>Reason</TD>
                <td>{{this.paramsObj.reason}}</td>
            </tr>
            <tr>
                <td style='text-align:right;'>Employee:</TD>
                <td>{{this.paramsObj.user}}</td>
            </tr>
            <tr>
                <td style='text-align:right;'>IP Address:</TD>
                <td>{{this.ipAddress}}</td>
            </tr>
        </table>
        <hr />
        <div v-if="this.hasSubmitted === false ? true : false">
            <p style='font-size:small; text-align:left;'>If you feel you have reached this page in error, and you think that
            you are attempting to access a legitimate business site, please submit an explanation for your inquiry and
            someone
            will review your request. If you need further assistance, contact the Help Desk at
            1-800-991-3441.<br /><br /></p>
            <form 
                action="https://script.google.com/macros/s/AKfycbxnvLy4AfRqhDczR0x6XIo5dgIPRkblH5Ry4G61i4oK41eN0cme/exec"
                method="POST">
                <div class="form-elements">
                    <textarea rows="10" cols="70" name="reason" required></textarea>
                    <input type="hidden" name="url" :value="this.paramsObj.url">
                    <input type="hidden" name="url-category" :value="this.params.cat">
                    <input type="hidden" name="reason" :value="this.paramsObj.reason">
                    <input type="hidden" name="employee" :value="this.paramsObj.user">
                    <input type="hidden" name="ip-address" :value="this.ip">
                    <br /><br />
                    <input type="submit" value="Submit" style="width: 7em;" @click=submit>
                </div>
            </form>
        </div>
            <div v-if="this.hasSubmitted === true ? true : false">
                    <!-- You can customize the thankyou message by editing the code below -->
                    <h2><em>Thanks</em> for contacting us! Check back here soon!
                    </h2>
                </div>
            <br /><br />
            <div style='text-align:center; font-weight:bold;'>Notice: Due to the continued current and emerging threat
                of email based attacks on the Fifth Third network, we have determined that access to web based email
                services will be prohibited indefinitely in accordance with the Electronic Communications Policy.</div>
    </div>

  </div>    
</template>

<script>
export default {
    props: {
       params: Array, 
       ip: String
    },
    data() {
        return {
            paramsObj: this.params,
            ipAddress: this.ip, 
            hasSubmitted: false
        }
    }, 
    methods: {
        submit: function() {
            this.hasSubmitted = !this.hasSubmitted
        }
    }
}
</script>

<style scoped>

</style>
