<template>
  <div class="container">
    <div v-if="!isErrorState()">
      <h1>{{ question }}</h1>

      <form>
        <div
          class="row justify-content-md-center"
          v-for="polldleOption in polldleOptions"
          :key="polldleOption.key"
        >
          <div class="col-4">
            <div class="radio">
              <label>
                <input
                  type="radio"
                  v-model="polldleOptionResponses"
                  :value="polldleOption.id.toString()"
                >
                {{ polldleOption.name }}
              </label>
            </div>
          </div>
        </div>
      </form>

      <div class="row">
        <div class="col">
          <button
            type="button"
            class="validate-button btn-lg btn-primary mx-auto d-block"
            @click="vote()"
          >Vote</button>
        </div>
      </div>
    </div>

    <div
      v-show="isErrorState() || isVoteErrorState()"
      class="error-message alert alert-danger"
      role="alert"
    >{{ errorMessage }}</div>
  </div>
</template>

<script>
import axios from 'axios';

const stateResult = {
  WAITING_VOTE: "waiting_vote",
  VOTE_ERROR: "vote_error",
  ERROR: "error"
};

export default {
  name: "VotePolldle",
  data() {
    return {
      state: null,
      errorMessage: "",
      question: "",
      polldleOptions: [],
      polldleOptionResponses: null
    };
  },
  created() {
    axios.get(process.env.VUE_APP_SERVER_URL + "/polldles", {
      params : {
        pathURL: this.$route.params.pathurl
      }
    }).then(response => {
      if (response.status === 200) {
        this.question = response.data.question;
        this.polldleOptions = response.data.polldleOptions;
        this.state = stateResult.WAITING_VOTE;
      } else {
        this.errorMessage = "Polldle can not be loaded.";
        this.state = stateResult.ERROR;
      }  
    }).catch(error => {
      this.errorMessage = "Polldle can not be loaded.";
      this.state = stateResult.ERROR;
      console.error(error);
    });
  },
  methods: {
    vote() {
      // Prepare the data.
      var polldleVote = {
        pathUrl: this.$route.params.pathurl,
        polldleOptionResponses: [this.polldleOptionResponses]
      };

      axios({
        method: 'post',
        baseURL: process.env.VUE_APP_SERVER_URL + "/polldles/" + this.$route.params.pathurl + "/votes",
        data: JSON.stringify(polldleVote),
        headers: { 
          'Content-Type': 'application/json'
        }
      }).then(response => {
        if (response.status === 200) {
          console.log(this.$route.params.pathurl);
          this.$router.push({
            name: "ResultPolldle",
            params: { pathurl: this.$route.params.pathurl }
          });
        } else if (response.status === 204) {
          this.state = stateResult.VOTE_ERROR;
          this.errorMessage = "Already voted !!!";
        }
      }).catch(() => {
        this.state = stateResult.VOTE_ERROR;
        this.errorMessage = "Problem to vote for this Polldle.";
      });
    },
    isWaitingVoteState() {
      return this.state === stateResult.WAITING_VOTE;
    },
    isVoteErrorState() {
      return this.state === stateResult.VOTE_ERROR;
    },
    isErrorState() {
      return this.state === stateResult.ERROR;
    }
  }
};
</script>

<style>
</style>
