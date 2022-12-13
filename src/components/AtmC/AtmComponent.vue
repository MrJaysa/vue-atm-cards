<template>
  <div class="wrapper">
    <!-- cards -->
    <div class="cards-container">
      <section class="cards">
        <!-- card front -->
        <div id="card-front">
          <img src="../../assets/images/card-logo.svg" alt="" class="logo" />
          <p class="card-no" data-placeholder="0000 0000 0000 0000">
            {{ state.cardholder_number }}
          </p>
          <div class="card-details">
            <p class="card-holder" data-placeholder="jane appleased">
              {{ state.cardholder_name }}
            </p>
            <p class="exp-date">
              <span class="mm" data-placeholder="00">{{ state.mm }}</span
              >/<span class="yy" data-placeholder="00">{{ state.yy }}</span>
            </p>
          </div>
        </div>
        <!-- card back -->
        <div id="card-back">
          <p class="cvc-no" data-placeholder="000">{{ state.cvc }}</p>
        </div>
      </section>

      <!-- form -->
      <section class="form">
        <!-- modal -->
        <PopUp
          v-if="popupTriggers.buttonTrigger"
          :TogglePopup="() => Togglepopup('buttonTrigger')"
        >
          <img src="../../assets/images/icon-complete.svg" alt="" />
          <h2>THANK YOU!</h2>
          <p class="pop">We've added your card details</p>
        </PopUp>
        <!-- modal end -->
        <form @submit="submitForm">
          <div class="cardholder-name">
            <label for="name">cardholder name</label>
            <input
              type="text"
              id="cardholder"
              v-model="state.cardholder_name"
              required
            />
          </div>
          <div class="cardholder-number">
            <label for="number">card number</label>
            <input
              type="text"
              id="cardnumber"
              v-maska="'#### #### #### ####'"
              v-model="state.cardholder_number"
            />
            <span class="error" v-if="v$.cardholder_number.$error">
              {{ v$.cardholder_number.$errors[0].$message }}
            </span>
          </div>

          <div class="exp-date2">
            <div class="month">
              <label for="exp">exp. year</label>
              <input type="text" class="mm" v-model="state.mm" maxlength="2" />
              <span class="error" v-if="v$.mm.$error">
                {{ v$.mm.$errors[0].$message }}
              </span>
            </div>
            <div class="year">
              <label for="year">(mm/yy)</label>
              <input type="text" class="yy" v-model="state.yy" maxlength="2" />
            </div>
            <div class="cvc">
              <label for="cvc">cvc</label>
              <input
                type="text"
                class="cvc"
                v-model="state.cvc"
                maxlength="3"
              />
              <span class="error cv" v-if="v$.cvc.$error">
                {{ v$.cvc.$errors[0].$message }}
              </span>
            </div>
          </div>
          <button>submit</button>
        </form>
      </section>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { maska } from "maska";
import PopUp from "../PopUp.vue";
import useValidate from "@vuelidate/core";
import {
  required,
  alphaNum,
  maxLength,
  minLength,
  helpers,
} from "@vuelidate/validators";
import { reactive, computed } from "vue";

export default {
  directives: { maska },
  setup() {
    const state = reactive({
      cardholder_name: "",
      cardholder_number: "",
      mm: "",
      yy: "",
      cvc: "",
    });

    const popupTriggers = ref({
      buttonTrigger: true,
    });

    const rules = computed(() => {
      return {
        cardholder_name: { required },
        cardholder_number: {
          required: helpers.withMessage("Can't be blank", required),
          // alphaNum: helpers.withMessage("Wrong format, numbers only", alphaNum),
          maxLength: maxLength(19),
          minLength: minLength(12),
        },
        mm: {
          required: helpers.withMessage("Can't be blank", required),
          alphaNum: helpers.withMessage("Wrong format, numbers only", alphaNum),
          maxLength: maxLength(2),
          minLength: minLength(2),
        },
        yy: {
          required: helpers.withMessage("Can't be blank", required),
          alphaNum: helpers.withMessage("Wrong format, numbers only", alphaNum),
          maxLength: maxLength(2),
          minLength: minLength(2),
        },
        cvc: {
          required: helpers.withMessage("Can't be blank", required),
          alphaNum: helpers.withMessage("Wrong format, numbers only", alphaNum),
          maxLength: maxLength(3),
          minLength: minLength(3),
        },
      };
    });

    const v$ = useValidate(rules, state);

    const Togglepopup = (trigger) => {
      popupTriggers.value[trigger] = !popupTriggers.value[trigger];
    };

    return {
      PopUp,
      popupTriggers,
      Togglepopup,
      v$,
      state,
    };
  },

  watch: {
    cardholder_number() {
      this.cardholder_number = this.cardholder_number.replaceState(
        /[^0-9]/g,
        ""
      );
    },
  },

  methods: {
    validationStatus: function (validation) {
      return typeof validation != "undefined" ? validation.$error : false;
    },

    submitForm(event) {
      event.preventDefault();
      this.v$.$validate();
      if (!this.v$.$error) {
        alert("Form Successfully submitted boss!!");
        this.Togglepopup("buttonTrigger");
      } else {
        alert("Form failed validation boss");
      }
    },
  },
  components: { PopUp },
};
// @click="() => Togglepopup('buttonTrigger')"
</script>

<style>
/* Font */
@import url("https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500&display=swap");
/* CSS Variables */
:root {
  --white: hsl(0, 0%, 100%);
  --LightGrayishViolet: hsl(270, 3%, 87%);
  --DarkGrayishViolet: hsl(279, 6%, 55%);
  --VeryDarkViolet: hsl(278, 68%, 11%);
  --shadow: 5px 5px 5px rgba(104, 104, 104, 0.2);
  --font: "Space Grotesk", sans-serif;
}

/* Global Styles */
* {
  margin: 0;
  padding: 0;
  /* overflow-x: hidden; */
}
html {
  box-sizing: border-box;
  font-family: var(--font);
  /* overflow-x: hidden; */
}
.wrapper {
  display: grid;
  gap: 20px;
  /* overflow-x: hidden; */
}
p {
  color: var(--white);
  font-size: 18px;
  text-transform: uppercase;
}
.pop {
  color: var(--LightGrayishViolet);
  margin-top: 2rem;
}
.card-holder:empty:not(:focus)::before {
  content: attr(data-placeholder);
}
.card-no:empty:not(:focus)::before {
  content: attr(data-placeholder);
}
.mm:empty:not(:focus)::before {
  content: attr(data-placeholder);
}
.yy:empty:not(:focus)::before {
  content: attr(data-placeholder);
}
.cvc-no:empty:not(:focus)::before {
  content: attr(data-placeholder);
}
/* error styles */
.error {
  width: 163px;
  color: red;
  margin-top: 5rem;
  font-size: 70%;
  position: absolute;
}
.cv {
  margin-left: 1.4rem;
}
.is-invalid {
  border: 1px solid red;
}

/* Cards Container*/
.cards-container {
  display: grid;
  gap: 20px;
  grid-template-areas: "cards form form";
}

/* Cards */
.cards {
  background: url(../../assets/images/bg-main-desktop.png);
  grid-area: cards;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  height: 47.5rem;
  display: flex;
  flex-direction: column;
  width: 100%;
  z-index: 100;
}
/* card front */
#card-front {
  background: url(../../assets/images/bg-card-front.png);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  min-height: 30%;
  width: 100%;
  padding: 1rem;
  transform: translateX(10rem) translateY(5rem);
  border-radius: 0.6rem;
  box-shadow: var(--shadow);
  display: grid;
  place-items: center;
}
.logo {
  transform: translateX(-9rem) translateY(-5rem);
}
.card-no {
  font-size: 180%;
  letter-spacing: 0.1rem;
  position: absolute;
  width: 85%;
  transform: translateY(2rem);
}
.card-details {
  position: absolute;
  display: flex;
  justify-content: space-between;
  transform: translateY(6rem);
}
.card-holder {
  margin-left: -12rem;
}
.exp-date {
  margin-right: -11rem;
}
.exp-date2 {
  /* position: absolute; */
  margin-right: -11rem;
  width: 30rem;
}
/* card back */
#card-back {
  background: url(../../assets/images/bg-card-back.png);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  min-height: 30%;
  width: 100%;
  padding: 1rem;
  transform: translateX(15rem) translateY(8rem);
  border-radius: 0.6rem;
  box-shadow: var(--shadow);
}
.cvc-no {
  transform: translateX(22rem) translateY(6.2rem);
  width: 8%;
}

/* form */
.form {
  grid-area: form;
  display: grid;
  place-items: center;
}
form {
  display: flex;
  flex-direction: column;
  width: 50%;
  margin-left: 10rem;
}
form label {
  text-transform: uppercase;
  font-size: 18px;
  color: var(--VeryDarkViolet);
  margin-bottom: 0.6rem;
  letter-spacing: 1px;
}
form input {
  height: 2rem;
  width: 60%;
  padding: 0.3rem;
  margin-bottom: 2rem;
  outline: none;
  font-size: 18px;
  color: var(--DarkGrayishViolet);
  border: 1px solid var(--LightGrayishViolet);
  border-radius: 0.6rem;
}
form input:focus {
  border: 1px solid var(--VeryDarkViolet);
}
form .cardholder-name {
  display: flex;
  flex-direction: column;
}
form .cardholder-number {
  display: flex;
  flex-direction: column;
}
form .exp-date2 {
  display: flex;
}
form .exp-date2 .year {
  display: flex;
  flex-direction: column;
}
form .exp-date2 .month {
  display: flex;
  flex-direction: column;
}
form .exp-date2 .cvc {
  display: flex;
  flex-direction: column;
}
form .exp-date2 .cvc label {
  margin-left: 1.3rem;
}
form .exp-date2 .cvc input {
  margin-left: 1.3rem;
  width: 55%;
}
form .exp-date2 input {
  width: 4rem;
}
button {
  padding: 0.5rem;
  background: var(--VeryDarkViolet);
  color: var(--white);
  font-size: 18px;
  border-radius: 0.4rem;
  width: 65%;
  cursor: pointer;
}

/* media queries */
@media (max-width: 500px) {
  .cards-container {
    grid-template-areas: "cards" "form";
  }
  .cards {
    background: url(../../assets/images/bg-main-mobile.png);
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    height: 20rem;
    width: 35.8rem;
  }
  #card-front {
    transform: translateX(3rem) translateY(11.5rem);
    width: 68%;
    min-height: 65%;
    z-index: 100;
  }
  #card-back {
    transform: translateX(8rem) translateY(-12rem);
    width: 68%;
    min-height: 65%;
  }
  .card-no {
    transform: translateX(-1rem) translateY(2rem);
  }
  .cvc-no {
    transform: translateX(21rem) translateY(5.6rem);
  }
  .form {
    margin-top: 10rem;
    place-items: flex-start;
    margin-left: -7rem;
  }
  form input {
    width: 135%;
    border: 2px solid var(--LightGrayishViolet);
    padding: 1rem;
  }
  form .exp-date2 input {
    width: 5rem;
  }
  form .exp-date2 {
    gap: 10px;
  }
  form .exp-date2 .cvc input {
    /* margin-left: 1.3rem; */
    width: 95%;
  }
  form button {
    width: 147%;
  }
}
</style>
