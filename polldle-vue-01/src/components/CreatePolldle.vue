<template>
  <div class="container">
    <!-- Titre + description -->
    <h1>PollDLE</h1>
    <h2>Voting done simply in real-time</h2>

    <!-- PollDLE name -->
    <div class="row">
      <div class="col">
        <input type="text" class="large-input mx-auto d-block" placeholder="Add your question here">
      </div>
    </div>

    <h3>Add your PollDLE options</h3>

    <div class="row">
      <div class="col">
        <input type="text" placeholder="Polldle Option" class="large-input mx-auto d-block">
      </div>
    </div>
    <div class="row">
      <div class="col">
        <button type="button" class="clear-button btn-lg btn-danger mx-auto d-block" >Clear all PollDLE Options</button>
      </div>
    </div>

    <!-- PollDLE option -->
    <div class="row justify-content-center"></div>

    <!-- Button Action -->
    <div class="row">
      <div class="col">
        <button type="button" class="validate-button btn-lg btn-primary mx-auto d-block">Create PollDLE</button>
      </div>
    </div>

    <div class="alert alert-primary" role="alert">
      <h4 class="alert-heading">Summary of your PollDLE</h4>
      <hr>
      <p>
        The question is: <strong>TODO</strong>
      </p>
      <p>Number of PollDLE options: TODO</p>
    </div>

    <div class="error-message alert alert-danger" role="alert">TODO</div>
  </div>
</template>
 
<script>
export default {
  name: "CreatePolldle",
  data() {
    return {
      question: "",
      newPolldleOptionText: "",
      polldleOptions: [],
      errorMessage: "",
      buttonShown: false
    };
  },

  methods: {
    removedPolldleOption(polldleOption) {
      let index = this.polldleOptions.indexOf(polldleOption);
      this.polldleOptions.splice(index, 1);
      this.errorMessage = "";
    },

    addPolldleOption() {
      this.polldleOptions.push({
        text: this.newPolldleOptionText
      });
      this.newPolldleOptionText = "";
    },

    clearAllPolldleOptions() {
      this.polldleOptions = [];
      this.errorMessage = "";
    },

    createPolldle() {
      var polldleObject = {
        question: this.question,
        polldleOptions: []
      };

      this.polldleOptions.forEach(element => {
        var newPollOptionElement = { name: element.text };
        if (element.text !== "") {
          polldleObject.polldleOptions.push(newPollOptionElement);
        }
      });

      // Call REST web service with fetch API
    },

    isCreatePolldleDisabled() {
      return (
        this.polldleOptions === null ||
        this.polldleOptions.length < 2 ||
        this.question === ""
      );
    }
  }

}
</script>

<style>
.large-input {
  box-sizing: border-box;
  width: 500px;
  max-width: 80%;
  border-radius: 7px;
  border: 1px solid #bdc3c7;
  padding: 10px 20px;
  outline: none;
  text-align: center;
  line-height: 42px;
  font-size: 15px;
  margin: 20px;
  margin-top: 0;
  font-size: 20px;
}

.clear-button {
  margin-bottom: 25px;
}

.error-message {
  font-size: 125%;
  font-weight: bold;
}
</style>