<template>
  <v-container>
    <v-row
      v-if="isScreenLarge && !dialog && !dialog2"
      class="d-flex justify-center px-12"
    >
      <v-col class="body pa-0 d-flex" cols="12" md="12" lg="9">
        <v-col
          cols="6"
          class="my-auto px-0 left pl-sm-8 pl-md-7 pl-lg-10 py-sm-0"
        >
          <v-col class="d-flex flex-column">
            <div class="mt-sm-0 mt-md-9 mt-lg-12">
              <div class="heading">Stay updated!</div>
            </div>
            <div class="para mt-sm-1 mb-sm-3 mt-md-2 mb-md-5">
              Join 60,000+ product managers receiving monthly updates on:
            </div>
            <div class="points">
              <div class="point1 d-flex">
                <div class="check">
                  <img src="/src/assets/icon-list.svg" alt="" />
                </div>
                <div class="point">
                  Product discovery and building what matters
                </div>
              </div>
              <div class="point2 d-flex">
                <div class="check">
                  <img src="/src/assets/icon-list.svg" alt="" />
                </div>
                <div class="point">
                  Measuring to ensure updates are a success
                </div>
              </div>
              <div class="point3 d-flex">
                <div class="check">
                  <img src="/src/assets/icon-list.svg" alt="" />
                </div>
                <div class="point">And much more!</div>
              </div>
            </div>
            <div v-if="!isSubscribed" class="subscription-form">
              <div class="bar d-flex justify-space-between mb-0">
                <div class="left2">Email address</div>
                <div class="right2">
                  <p v-if="emailError" class="error-message">
                    Valid email required
                  </p>
                </div>
              </div>
              <input
                type="email"
                v-model="email"
                placeholder="email@company.com"
                :class="[emailError ? 'email-input-error' : 'email-input']"
              />

              <button
                @click="subscribe"
                class="subscribe-btn mt-0 mt-sm-1 mt-md-2"
              >
                Subscribe to monthly newsletter
              </button>
            </div>
          </v-col>
        </v-col>
        <v-col cols="6" class="px-0 right d-flex justify-center align-center">
          <img
            class="img my-2"
            src="/src/assets/illustration-sign-up-desktop.svg"
            alt=""
          />
        </v-col>
      </v-col>
    </v-row>
    <v-row v-if="!isScreenLarge && !dialog && !dialog2">
      <v-col cols="12" class="d-flex flex-column pa-0 body10">
        <div class="img10 d-flex justify-center align-center">
          <img
            src="/src/assets/illustration-sign-up-mobile.svg"
            alt="Sign Up"
          />
        </div>
        <div class="px-7">
          <div class="">
            <div class="heading mt-9 mb-5">Stay updated!</div>
          </div>
          <div class="para10 mt-sm-1 mb-sm-3 mt-md-2 mb-md-5">
            Join 60,000+ product managers receiving monthly updates on:
          </div>
          <div class="points mt-6">
            <div class="point1 d-flex">
              <div class="check">
                <img src="/src/assets/icon-list.svg" alt="" />
              </div>
              <div class="point10">
                Product discovery and building what matters
              </div>
            </div>
            <div class="point2 d-flex">
              <div class="check">
                <img src="/src/assets/icon-list.svg" alt="" />
              </div>
              <div class="point10">
                Measuring to ensure updates are a success
              </div>
            </div>
            <div class="point3 d-flex">
              <div class="check">
                <img src="/src/assets/icon-list.svg" alt="" />
              </div>
              <div class="point10">And much more!</div>
            </div>
          </div>
          <div v-if="!isSubscribed" class="subscription-form">
            <div class="bar d-flex justify-space-between mb-0">
              <div class="left20">Email address</div>
              <div class="right2">
                <p v-if="emailError" class="error-message">
                  Valid email required
                </p>
              </div>
            </div>
            <input
              type="email"
              v-model="email"
              placeholder="email@company.com"
              :class="[emailError ? 'email-input-error' : 'email-input']"
            />

            <button
              @click="subscribe2"
              class="subscribe-btn mt-0 mt-sm-1 mt-md-2"
            >
              Subscribe to monthly newsletter
            </button>
          </div>
        </div>
      </v-col>
    </v-row>
  </v-container>

  <v-dialog
    :scrim="false"
    v-model="dialog"
    persistent
    max-width="446"
    class="custom-dialog"
  >
    <v-card class="custom-card px-16" rounded="xl">
      <div class="img5 mt-6 mb-8">
        <img src="/src/assets/icon-success.svg" alt="" />
      </div>
      <div class="headline px-0 mb-5">Thanks for subscribing!</div>
      <div class="para5">
        A confirmation email has been sent to
        <span class="email-success">{{ email }}</span> Please open it and click
        the button inside to confirm your subscription.
      </div>
      <v-card-actions class="custom-actions">
        <v-btn @click="closeDialog" class="custom-btn py-6 mt-7 mb-7" block>
          Dismiss message
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-dialog
    :scrim="false"
    v-model="dialog2"
    persistent
    fullscreen
    class="custom-dialog2"
  >
    <v-card class="custom-card2 px-16" rounded="xl">
      <div class="img5 mt-6 mb-8">
        <img src="/src/assets/icon-success.svg" alt="" />
      </div>
      <div class="headline px-0 mb-5">Thanks for subscribing!</div>
      <div class="para5">
        A confirmation email has been sent to
        <span class="email-success">{{ email }}</span> Please open it and click
        the button inside to confirm your subscription
      </div>
      <v-spacer></v-spacer>
      <v-card-actions class="custom-actions">
        <v-btn @click="closeDialog2" class="custom-btn py-6 mt-7 mb-7" block>
          Dismiss message
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
  
  <script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
//import { errorMessages } from "vue/compiler-sfc";
const isScreenLarge = ref(false);

// Function to update screen size status
const updateScreenSize = () => {
  isScreenLarge.value = window.matchMedia("(min-width: 599.5px)").matches;
};

// Attach the event listener to monitor screen size changes
onMounted(() => {
  updateScreenSize(); // Set the initial value
  window.addEventListener("resize", updateScreenSize);
});

// Cleanup the event listener on unmount
onBeforeUnmount(() => {
  window.removeEventListener("resize", updateScreenSize);
});
// Reactive state variables
const email = ref("");
const emailError = ref(false);
const dialog = ref(false);
const dialog2 = ref(false);
const isSubscribed = ref(false);

// Function to validate and handle subscription
const subscribe = () => {
  const emailPattern = /^[\w-\.]+@([\w-]+\.)+(com|co)$/;
  emailError.value = !emailPattern.test(email.value);

  if (!emailError.value) {
    isSubscribed.value = true;
    dialog.value = true;
  }
};
const subscribe2 = () => {
  const emailPattern = /^[\w-\.]+@([\w-]+\.)+(com|co)$/;
  emailError.value = !emailPattern.test(email.value);

  if (!emailError.value) {
    isSubscribed.value = true;
    dialog2.value = true;
  }
};

// Function to close the dialog and reset the form
const closeDialog = () => {
  dialog.value = false;
  isSubscribed.value = false; // Show the main template again
  email.value = ""; // Clear the input field
  emailError.value = false; // Reset the error state
};
const closeDialog2 = () => {
  dialog2.value = false;
  isSubscribed.value = false; // Show the main template again
  email.value = ""; // Clear the input field
  emailError.value = false; // Reset the error state
};
</script>
  
  <style scoped>
@font-face {
  font-family: f1;
  src: url(/src/assets/Roboto-Bold.ttf);
}

@font-face {
  font-family: f2;
  src: url(/src/assets/Roboto-Regular.ttf);
}
.heading {
  color: hsl(237, 25%, 18%);
  font-size: 55px;
  font-family: f1;
}
.para,
.point {
  color: hsl(237, 25%, 18%);
  font-family: f2;
  font-size: 15px;
}
.body {
  background-color: #fff;
  border-radius: 20px;
}
.img {
  width: 86%;
  position: relative;
  right: -8px;
}
.subscription-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  margin: 50px auto;
}

.email-input {
  padding: 10px 10px 10px 20px;
  border: 1.5px solid #ccc;
  border-radius: 5px;
  outline: none;
  width: 100%;
  font-family: f2;
  color: hsl(232, 31%, 20%);
}
.email-input-error {
  padding: 10px 10px 10px 20px;
  border: 1.5px solid hsl(4, 100%, 67%);
  border-radius: 5px;
  outline: none;
  width: 100%;
  background-color: hsl(7, 100%, 95%);
  color: hsl(4, 100%, 67%);
  font-family: f2;
}
.email-input:hover {
  border: 1.5px solid hsl(257, 3%, 48%);
  outline: none;
}
.error-message {
  color: hsl(4, 100%, 67%);
  font-size: 11px;
  font-family: f1;
}

.subscribe-btn {
  padding: 10px;
  background-color: hsl(232, 31%, 20%);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-family: f2;
  font-weight: 550;
  font-size: 14px;
}
.subscribe-btn:hover {
  background: linear-gradient(90deg, hsl(346, 99%, 66%), hsl(14, 100%, 62%));
  box-shadow: 0 0 5px hsl(346, 99%, 66%), 0 0 5px hsl(14, 100%, 62%);
}
.email-input::placeholder {
  color: hsl(228, 3%, 70%); /* Dark grayish tone */
  font-family: f2; /* Custom font */
  font-size: 14px;
  opacity: 1; /* Ensures full color visibility on all browsers */
  /* padding-left: 10px; */
}
.email-input-error::placeholder {
  color: hsl(228, 3%, 70%); /* Dark grayish tone */
  font-family: f2; /* Custom font */
  font-size: 14px;
  opacity: 1; /* Ensures full color visibility on all browsers */
  /* padding-left: 10px; */
}

.check {
  margin-right: 13px;
}
.check img {
  scale: 0.8;
}
.left2 {
  font-size: 10px;
  font-family: f1;
  color: hsl(235, 13%, 17%);
}

/* DIALOG BOX */
.custom-card {
  background-color: rgb(255, 255, 255);
  padding: 20px;
}

.custom-actions {
  width: 100%;
  justify-content: center;
}

.custom-btn {
  width: 100%; /* Make it full width */
  font-size: 14px; /* Adjust font size if needed */
  background-color: hsl(232, 31%, 20%);
  color: white;
  letter-spacing: 0.8px;
}
.custom-btn:hover {
  background: linear-gradient(90deg, hsl(346, 99%, 66%), hsl(14, 100%, 62%));
  box-shadow: 0 0 5px hsl(346, 99%, 66%), 0 0 5px hsl(14, 100%, 62%);
}
.headline {
  font-size: 50px;
  font-family: f1;
  line-height: 50px;
  color: hsl(235, 26%, 18%);
}
.email-success {
  font-family: f1;
  color: hsl(237, 14%, 25%);
  font-size: 14px;
  font-weight: 900;
}
.para5 {
  color: hsl(237, 14%, 25%);
  font-size: 14px;
  font-weight: 500;
}
/* MEDIA QUERY */
@media screen and (min-width: 600px) and (max-width: 749.49px) {
  .heading {
    font-size: 30px;
  }
  .para,
  .point {
    font-size: 12px;
  }
  .subscription-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    margin: 20px auto 0px auto;
  }
  .subscribe-btn {
    padding: 7px;
    background-color: hsl(232, 31%, 20%);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-family: f2;
    font-weight: 550;
    font-size: 12px;
  }
  .email-input {
    padding: 5px 10px 5px 15px;
    border: 1.5px solid #ccc;
    border-radius: 5px;
    outline: none;
    width: 100%;
    font-family: f2;
    color: hsl(232, 31%, 20%);
    font-size: 12px;
  }
  .email-input-error {
    font-size: 12px;
    padding: 5px 10px 5px 15px;
    border: 1.5px solid hsl(4, 100%, 67%);
    border-radius: 5px;
    outline: none;
    width: 100%;
    background-color: hsl(7, 100%, 95%);
    color: hsl(4, 100%, 67%);
  }
}

@media screen and (min-width: 750px) and (max-width: 849.5px) {
  .heading {
    font-size: 42px;
  }
  .para,
  .point {
    font-size: 14px;
  }
  .subscription-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    margin: 20px auto 0px auto;
  }
  .subscribe-btn {
    padding: 7px;
    background-color: hsl(232, 31%, 20%);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-family: f2;
    font-weight: 550;
    font-size: 14px;
  }
  .email-input {
    padding: 10px 10px 10px 15px;
    border: 1.5px solid #ccc;
    border-radius: 5px;
    outline: none;
    width: 100%;
    color: hsl(232, 31%, 20%);
    font-size: 14px;
  }
  .email-input-error {
    font-size: 14px;
    padding: 10px 10px 10px 15px;
    border: 1.5px solid hsl(4, 100%, 67%);
    border-radius: 5px;
    outline: none;
    width: 100%;
    background-color: hsl(7, 100%, 95%);
    color: hsl(4, 100%, 67%);
  }
  .subscribe-btn {
    padding: 12px;
    background-color: hsl(232, 31%, 20%);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-family: f2;
    font-weight: 550;
    font-size: 14px;
  }
  .subscription-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    margin: 35px auto 0px auto;
  }
}
@media screen and (min-width: 850px) and (max-width: 959.5px) {
  .heading {
    font-size: 50px;
  }
  .para,
  .point {
    font-size: 14px;
  }
  .subscription-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    margin: 20px auto 0px auto;
  }
  .subscribe-btn {
    padding: 7px;
    background-color: hsl(232, 31%, 20%);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-family: f2;
    font-weight: 550;
    font-size: 14px;
  }
  .email-input {
    padding: 10px 10px 10px 15px;
    border: 1.5px solid #ccc;
    border-radius: 5px;
    outline: none;
    width: 100%;
    color: hsl(232, 31%, 20%);
    font-size: 14px;
  }
  .email-input-error {
    font-size: 14px;
    padding: 10px 10px 10px 15px;
    border: 1.5px solid hsl(4, 100%, 67%);
    border-radius: 5px;
    outline: none;
    width: 100%;
    background-color: hsl(7, 100%, 95%);
    color: hsl(4, 100%, 67%);
  }
  .subscribe-btn {
    padding: 12px;
    background-color: hsl(232, 31%, 20%);
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-family: f2;
    font-weight: 550;
    font-size: 14px;
  }
  .subscription-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    margin: 35px auto 0px auto;
  }
}

/* MOBILE SCREEN */
.body10 {
  background-color: #fff;
}
.img10 {
  width: 100%; /* Full width of the container */
  overflow: hidden; /* Prevent overflow */
}

.img10 img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensures the image fills the div without distortion */
}
.para10,
.point10 {
  color: hsl(237, 25%, 18%);
  font-family: f2;
  font-size: 16px;
  font-weight: 500;
}
.left20 {
  font-size: 14px;
  font-family: f1;
  color: hsl(235, 13%, 17%);
}
/* DIALOG BOX */
.custom-dialog2
{
  background-color: #ffffff;
}
.custom-card2 {
  background-color: rgb(255, 255, 255);
  padding: 20px;
}
</style>
  