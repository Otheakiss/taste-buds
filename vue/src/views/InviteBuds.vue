<template>
  <div id="invite-buds">
    <div class="tab-buttons">
      <button
        type="button"
        id="step-1-create-event"
        class="button"
        v-on:click="showFormStepOne()"
      >
        Step 1: Info
      </button>
      <button
        type="button"
        id="step-2-create-event"
        class="button"
        v-on:click="showFormStepTwo()"
      >
        Step 2: Tastes
      </button>
      <button
        type="button"
        id="step-3-create-event"
        class="button"
        v-on:click="showFormStepThree()"
      >
        Step 3: Buds
      </button>
     
      <button
        type="button"
        id="create-event-button"
        class="button"
        v-on:click="createEvent()"

      >
        Submit
      </button>
    </div>
    <!-----------------------Event Form----------------------->

    <!-- <form id="event-form" name="frm" @submit="isEmpty()"> -->
      <form id="event-form" name="frm" @submit="createEvent()">

      <h1>Enter your event information below:</h1>

      <div class="event-info">
        <h2>Event Name:</h2>
        <input
          type="text"
          class="event-input"
          name="event-in"
          v-model="event.eventName"
          placeholder="your event name"
        />
        <h2>Event Date:</h2>
        <input
          type="date"
          class="event-input"
          name="date-in"
          v-model="event.eventDate"
          placeholder="mm/dd/yyyy"
        />
        <h2>Event Time:</h2>
        <input
          type="time"
          class="event-input"
          name="time-in"
          v-model="event.eventTime"
          placeholder="hh:mm"
        />
      </div>

      <div class="invite-due-date-time">
        <h2>When must your buds respond by?</h2>
        <input
          type="date"
          class="event-input"
          name="due-date"
          v-model="event.deadlineDate"
          placeholder="mm/dd/yyyy"
        />
        <br />
        <input
          type="time"
          class="event-input"
          name="due-time"
          v-model="event.deadlineTime"
          placeholder="hh:mm"
        />
      </div>
    </form>

    <!-----------------------Invite Form----------------------->

    <form
      id="invite-form"
      ref="invite-form"
      v-show="false"
      @submit="addToInvitees()"
    >
      <div class="email">
        <h1>Enter the email address of the bud you want to invite:</h1>
        <input
          type="text"
          id="invitee-input"
          class="date"
          v-model="invitation.emailAddress"
          placeholder="your bud's email"
          @keydown.enter.exact.prevent="addToInvitees()"
        />
        <button
          type="button"
          id="invite-button"
          v-on:click="addToInvitees()"
        >
          ADD
        </button>
      </div>

      <div class="send-invite"></div>

      <h2>Below is a list of current invitees to this event:</h2>
      <div
        id="current-list-of-invitees"
        v-for="invitation in invitees"
        :key="invitation.emailAddress"
      >
        <h2 id="inviteeEmailAddress">{{ invitation.emailAddress }}</h2>
      </div>
    </form>

      <!-------------------Add Restaurants to Event ----------------------->

    <form id="restaurants-form" v-show=false>  

    <div id="find" class="left-panel">
        <form class="find-form" @submit="find()">
          <br>
          <p>Enter a city or zipcode</p>
          <br>
            <input id="city-or-zip" type="text" class="location" v-model="location"
              @keydown.enter.exact.prevent="find()" placeholder="City or Zipcode"/>
            <br>
            <br>
            <label for="state">If you'd like, you can specify a state.</label>
            <br>
            <select id="state" name="state" v-model="state" size="5"> 
	<option value=",AL">Alabama</option>
	<option value=",AK">Alaska</option>
	<option value=",AZ">Arizona</option>
	<option value=",AR">Arkansas</option>
	<option value=",CA">California</option>
	<option value=",CO">Colorado</option>
	<option value=",CT">Connecticut</option>
	<option value=",DE">Delaware</option>
	<option value=",DC">District Of Columbia</option>
	<option value=",FL">Florida</option>
	<option value=",GA">Georgia</option>
	<option value=",HI">Hawaii</option>
	<option value=",ID">Idaho</option>
	<option value=",IL">Illinois</option>
	<option value=",IN">Indiana</option>
	<option value=",IA">Iowa</option>
	<option value=",KS">Kansas</option>
	<option value=",KY">Kentucky</option>
	<option value=",LA">Louisiana</option>
	<option value=",ME">Maine</option>
	<option value=",MD">Maryland</option>
	<option value=",MA">Massachusetts</option>
	<option value=",MI">Michigan</option>
	<option value=",MN">Minnesota</option>
	<option value=",MS">Mississippi</option>
	<option value=",MO">Missouri</option>
	<option value=",MT">Montana</option>
	<option value=",NE">Nebraska</option>
	<option value=",NV">Nevada</option>
	<option value=",NH">New Hampshire</option>
	<option value=",NJ">New Jersey</option>
	<option value=",NM">New Mexico</option>
	<option value=",NY">New York</option>
	<option value=",NC">North Carolina</option>
	<option value=",ND">North Dakota</option>
	<option value=",OH">Ohio</option>
	<option value=",OK">Oklahoma</option>
	<option value=",OR">Oregon</option>
	<option value=",PA">Pennsylvania</option>
	<option value=",RI">Rhode Island</option>
	<option value=",SC">South Carolina</option>
	<option value=",SD">South Dakota</option>
	<option value=",TN">Tennessee</option>
	<option value=",TX">Texas</option>
	<option value=",UT">Utah</option>
	<option value=",VT">Vermont</option>
	<option value=",VA">Virginia</option>
	<option value=",WA">Washington</option>
	<option value=",WV">West Virginia</option>
	<option value=",WI">Wisconsin</option>
	<option value=",WY">Wyoming</option>
              </select>
            <br>
            <button type="button" id="submit-button" v-on:click="find()">Search</button>

        </form>
    </div>

    <div id="find-restaurants-results" class="right-panel">
          <h2 id="search-directions">Search, and then click the knife and fork icon to add a restaurant.</h2>
        <restaurant-event-view class="card" v-for="business in businesses" v-bind:key="business.id" v-bind:business="business"> </restaurant-event-view>
    </div>

    </form>

    </div>

</template>

<script>
import RestaurantEventView from "../components/RestaurantEventView.vue";
import RestaurantService from "../services/RestaurantService.js";
import EventService from "../services/EventService.js";
import InvitationService from "../services/InvitationService.js";
// import emailjs from '@emailjs/browser';

export default {
  name: "create-event",
  components: {
    RestaurantEventView,
  },
  props: ["business"],
  data() {
    return {
      location: "",
      businesses: [],
      userId: "",
      event: {
        eventId: 0,
        eventName: "",
        eventDate: "",
        eventTime: "",
        eventOrganizerId: 0,
        deadlineDate: "",
        deadlineTime: "",
      },
      state: "",
      uniqueLink: "",
      invitation: {
        invitationId: "",
        eventId: 0,
        emailAddress: "",
      },
      invitationIds: [],
      invitees: [],
      eventRestaurants: [],
      eventRestaurant: {
        yelpRestaurantId: '',
        eventId: 0,
        voteCount:0
        },
    };
  },
  methods: {
    createEvent() {
      let eventNameInput = document.forms["frm"]["event-in"].value;
      let eventDateInput = document.forms["frm"]["date-in"].value;
      let eventTimeInput = document.forms["frm"]["time-in"].value;
      let eventDueDateInput = document.forms["frm"]["due-date"].value;
      let eventDueTimeInput = document.forms["frm"]["due-time"].value;
    
        if (eventNameInput == null || eventNameInput == "" || 
        eventDateInput == null || eventDateInput == "" || 
        eventTimeInput == null || eventTimeInput == "" ||
        eventDueDateInput == null || eventDueDateInput == "" ||
        eventDueTimeInput == null || eventDueTimeInput == "" ||
        this.$store.state.restaurants.length === 0 ||
        this.invitees.length === 0
        ) {
        alert("All Fields Must Be Filled");
        return false;
        } 

      this.eventDate = this.moment(this.eventDate).format("YYYY-MM-DD");
      this.deadlineDate = this.moment(this.deadlineDate).format("YYYY-MM-DD");
      EventService.createEvent(this.event).then((response) => {
        this.event.eventId = response.data.eventId;
        // console.dir(this.$store.state.restaurants);
        this.$store.state.restaurants.forEach((id) => {
          this.eventRestaurant.yelpRestaurantId = id;
          this.eventRestaurant.eventId = this.event.eventId;
          // console.log(this.eventRestaurant);
          RestaurantService.createEventRestaurantInDatabase({...this.eventRestaurant});
        });
        this.$store.state.invitees = [];
        this.$store.state.invitees = this.invitees;
        this.invitees.forEach((invitation) => {
          invitation.eventId = this.event.eventId;
          InvitationService.createInvitation(invitation).then((response) => {
            invitation.invitationId = response.data.invitationId;
            this.uniqueLink =
              "http://localhost:9000/invite-options/" + invitation.invitationId;
            this.$store.state.inviteeLinks.push(this.uniqueLink);
            })
          })
        })
        this.$router.push({name: 'confirmation'}); 
    },

    showFormStepOne() {
      const stepOneForm = document.getElementById("event-form");
      const stepTwoForm = document.getElementById("restaurants-form");
      const stepThreeForm = document.getElementById("invite-form");
      const buttonOne = document.getElementById("step-1-create-event");
      const buttonTwo = document.getElementById("step-2-create-event");
      const buttonThree = document.getElementById("step-3-create-event");
      stepOneForm.style.display = "block";
      stepTwoForm.style.display = "none";
      stepThreeForm.style.display = "none";
      buttonOne.style.backgroundColor = "#b1b1b1";
      buttonTwo.style.backgroundColor = "#f0efef";
      buttonThree.style.backgroundColor = "#f0efef";
      buttonOne.style.color = "white";
      buttonTwo.style.color = "#666666";
      buttonThree.style.color = "#666666";
    },

    showFormStepTwo() {
      const stepOneForm = document.getElementById("event-form");
      const stepTwoForm = document.getElementById("restaurants-form");
      const stepThreeForm = document.getElementById("invite-form");
      const buttonOne = document.getElementById("step-1-create-event");
      const buttonTwo = document.getElementById("step-2-create-event");
      const buttonThree = document.getElementById("step-3-create-event");
      stepOneForm.style.display = "none";
      stepTwoForm.style.display = "block";
      stepThreeForm.style.display = "none";
      buttonOne.style.backgroundColor = "#f0efef";
      buttonTwo.style.backgroundColor = "#b1b1b1";
      buttonThree.style.backgroundColor = "#f0efef";
      buttonOne.style.color = "#666666";
      buttonTwo.style.color = "white";
      buttonThree.style.color = "#666666";
    },

    showFormStepThree() {
      const stepOneForm = document.getElementById("event-form");
      const stepTwoForm = document.getElementById("restaurants-form");
      const stepThreeForm = document.getElementById("invite-form");
      const buttonOne = document.getElementById("step-1-create-event");
      const buttonTwo = document.getElementById("step-2-create-event");
      const buttonThree = document.getElementById("step-3-create-event");
      stepOneForm.style.display = "none";
      stepTwoForm.style.display = "none";
      stepThreeForm.style.display = "block";
      buttonOne.style.backgroundColor = "#f0efef";
      buttonTwo.style.backgroundColor = "#f0efef";
      buttonThree.style.backgroundColor = "#b1b1b1";
      buttonOne.style.color = "#666666";
      buttonTwo.style.color = "#666666";
      buttonThree.style.color = "white";
    },

    addToInvitees() {
      this.invitees.push({ ...this.invitation });
      this.invitation.emailAddress = "";
    },

    viewRestaurants() {
      return RestaurantService.findBusinessesByEventId(this.event.eventId).then(
        (response) => {
          this.businesses = response.data;
        }
      );
    },

    find() {
      if (this.state != undefined) {
        this.location = this.location + this.state;
      }
      RestaurantService.find(this.location).then((response) => {
        this.businesses = response.data;
        console.dir(this.businesses);
      });
    }
    },

  created() {
    this.event.eventOrganizerId = this.$store.state.user.id;
  }
}
</script>

<style scoped>
#event-form,
#invite-form {
  padding-top: 45px;
}

h1 {
  font-family: Montserrat;
  color: #666666;
  font-weight: normal;
  text-align: center;
  padding-top: 15px;
}

#step-1-create-event {
  background-color: #b1b1b1;
  color: white;
}

a.router-link-active {
  font-weight: bold;
}

input {
  width: 350px;
  align-self: center;
  padding: 10px 15px;
  border: 3px solid lightgray;
  text-align: center;
  justify-content: center;
  border-radius: 10px;
  font-size: 16px;
  display: flex;
  flex-grow: 0;
  margin-top: 15px;
}

h2 {
  text-align: center;
  font-family: Montserrat;
  font-weight: normal;
}

.tab-buttons {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.button {
  background-color: #f0efef;
  color: #666666;
  border: none;
  text-decoration: none;
  font-size: 22px;
  font-weight: normal;
  font-family: Montserrat;
  border-radius: 10px;
  width: 15%;
  padding: 12px 12px;
  margin-top: 40px;
  display: flex;
}

#create-event-button {
  background-color: #a64d79ff;
  color: white;
}

#invite-button,
#restaurant-button {
  background-color: #a64d79ff;
  color: white;
  border: none;
  text-decoration: none;
  font-size: 15px;
  font-weight: bold;
  border-radius: 10px;
  width: 15%;
  height: 45px;
  padding: 12px 12px;
}

#search-directions {
  padding-top: 40px;
}

placeholder {
  font-family: Montserrat;
  color: #909090;
}

input.date {
  justify-content: center;
  display: flex;
  align-content: center;
  margin: auto;
  margin-top: 60px;
}

#invite-button {
  justify-content: center;
  display: flex;
  align-content: center;
  margin: auto;
  margin-top: 20px;
  margin-bottom: 40px;
  width: 15%;
}

.event-input {
  display: flex;
  justify-content: center;
  margin: auto;
  font-family: Arial;
  color: #909090;
}

#event-info-button {
  width: 30%;
  margin: 10px;
}

#event-info-button:focus,
#invite-button:focus,
#restaurant-button:focus {
  background: #e06666;
}

#event-info-button:hover,
#invite-button:hover,
#restaurant-button:hover {
  background: #741b47ff;
}

body {
  padding-top: 15px;
  display: flex;
  order: bottom;
  z-index: -1;
}

.left-panel {
  width: 30%;
  display: flex;
  margin: auto;
  justify-content: center;
  margin-top: 20px;
}

form.find-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.right-panel {
  width: 100%;
  justify-content: center;
  text-align: center;
}

p {
  font-size: 25px;
  display: flex;
  line-height: 5px;
  font-family: Montserrat;
  font-weight: normal;
}

input.location {
  width: 175px;
  padding: 10px 15px;
  border: 3px solid lightgray;
  text-align: center;
  border-radius: 10px;
  font-size: 16px;
}

#submit-button {
  background-color: #a64d79ff;
  color: white;
  border: none;
  text-decoration: none;
  font-size: 15px;
  font-weight: bold;
  border-radius: 10px;
  width: 100px;
  padding: 12px 12px;
}

label {
  font-family: Montserrat;
  font-size: 23px;
}

select {
  -webkit-appearance: none;
  border: none;
  font-family: Montserrat;
  color: #666666;
  font-size: 20px;
  padding: 0 15px 0 0;
}

option {
  padding: 5px;
}

#submit-button:focus {
  background: #e06666;
}

#submit-button:hover {
  background: #741b47ff;
}

@media screen and (max-width: 1200px) {
  .container {
    padding-top: 15px;
    display: flex;
    flex-direction: column;
  }

  .left-panel {
    width: 30%;
    top: 175px;
    margin-top: 50px;
  }

  .right-panel {
    width: 100%;
    justify-content: center;
    text-align: center;
  }
}

@media screen and (max-width: 800px) and (min-width: 200px) {
  .container {
    display: flex;
  }

  .left-panel {
    width: 100%;
    padding-bottom: 20px;
  }

  .tab-buttons {
    flex-direction: row;
    margin-top: 25px;
  }

  .button {
    flex-grow: 1;
    width: 100%;
    margin: 5px;
  }

  .right-panel {
    width: 100%;
    justify-content: center;
    text-align: center;
  }
}
</style>