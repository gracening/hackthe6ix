<template>
	<div id="admin">
		<div class="wrap">
		<h1>Admin</h1>
		<div class="auth" v-show="!authenticated">
		<input type="text" v-model="password" placeholder="PASSWORD"/>
		<div class="btn" v-on:click="auth()">AUTHENTICATE</div>
		</div>
		<div class="message"><h4>{{message}}</h4></div>
    <div class="stats" v-show="authenticated">
      Applicants: {{validApplicants}}<br>
      Accepted: {{acceptedApplicants}}<br>
      Signed In: {{signedInApplicants}}<br>
      Rejected: {{rejectedApplicants}}<br>
      Waitlist: {{waitlistApplicants}}<br>
      RSVP: {{rsvpApplicants}}
    </div>
		<div class="applicants">
			<applicant v-for="applicant in applicants" :applicant="applicant" :password="password" v-bind:key="applicant.id"></applicant>
		</div>
		</div>
	</div>
</template>
<script>
import Applicant from './Applicant'
export default {
  name: 'admin',
  components: {
  	Applicant
  },
  data () {
    return {
    	password: '',
    	authenticated: false,
    	message: '',
    	applicants: []
    }
  },
  methods: {
  	auth() {
  		console.log(this.password)
  		this.authenticated = true;
  		this.fetch()
  	},
  	fetch() {
  		this.$http.get('https://ht6.lyninx.com/applicants?password='+this.password).then(res => {
  			if(res.body.success){
  				console.log(res)
  				this.applicants = res.body.applicants
  			} else {
  			    this.message = res.body.msg
  			}
  		});
  	}
  },
  computed: {
    validApplicants() {
      const valid = this.applicants.filter((applicant) => {
        return applicant.acceptedStatus != 'invalid'
      })
      return valid.length
    },
    acceptedApplicants() {
      const accepted = this.applicants.filter((applicant) => {
        return applicant.acceptedStatus == 'accepted'
      })
      return accepted.length
    },
    rejectedApplicants() {
      const rejected = this.applicants.filter((applicant) => {
        return applicant.acceptedStatus == 'rejected'
      })
      return rejected.length
    },
    signedInApplicants() {
      const signedIn = this.applicants.filter((applicant) => {
        return applicant.acceptedStatus == 'signed-in'
      })
      return signedIn.length
    },
    waitlistApplicants() {
      const waitlist = this.applicants.filter((applicant) => {
        return applicant.acceptedStatus == 'waitlist'
      })
      return waitlist.length
    },
    rsvpApplicants() {
      const rsvp = this.applicants.filter((applicant) => {
        return applicant.rsvp
      })
      return rsvp.length
    }
  }
}	
</script>
<style scoped>
#admin {
	color:#fff;
}
.main {
  padding-top:64px;
/*  background:#273355;*/
}
.stats {
  font-size:20px;
  margin-bottom:20px;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #eee;
  background-image:url("../assets/stars.svg");
  display:flex;
  flex-direction:column;
  justify-content:space-between;
  min-height:100vh;
}
.auth {
	display:inline-block;
	padding:20px;
	background: rgba(0,0,0,0.2);
}
.section {
  max-width: 900px;
  padding:20px;
  margin:auto;
}
.wrap {
  max-width:1024px;
  margin:auto;
  margin-bottom:0;
  padding:20px;
}
.row {
  display:flex;
  margin-bottom:8px;
}
.row .label {
  min-width:200px;
}
.row input {
  padding:6px;
  border:0;
  flex-grow:1;
}
.section {
  margin: auto;
  text-align:left;
  color:#fff;
}
.card {
  border:1px solid #fff;
  border-radius:8px;
  background-color: rgba(17, 26, 52, 0.6);
  padding:24px 40px;
  margin-bottom:24px;
  line-height:140%;
  overflow:hidden;
}
.btn {
  background: #E3493B;
  border-radius:12px;
  padding:4px 10px;
  color:#fff;
  display:inline-block;
  cursor:pointer;
}
p {
  margin-bottom:24px;
}
a {
  color:#555;
}
a:hover {
  color:#000;
  cursor:pointer;
}

/* transitions */
.fade-enter-active, .fade-leave-active {
  transition: all .4s ease-in-out;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
