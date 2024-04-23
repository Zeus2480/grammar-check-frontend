<template>
  <div class="tw-bg-[#000] tw-w-full tw-h-screen">
    <div
      style="border-bottom: 1px solid rgb(34, 34, 34)"
      class="header tw-bg-[#0a0a0a] tw-border-b-1 tw-py-4 tw-px-8 tw-border-gray-800 tw-border-solid"
    >
      <h4 class="tw-text-white tw-font-medium tw-text-xl boujee-text">
        Grammar Checker
      </h4>
    </div>
    <v-container>
      <v-row>
        <v-col cols="6">
          <div
            style="border: 1px solid rgb(34, 34, 34)"
            class="tw-m-8 tw-flex tw-items-center tw-p-8 tw-rounded tw-min-h-[70vh]"
          >
            <div class="tw-w-full">
              <h4 class="tw-my-4 tw-text-xl">
                Enter your text for grammar check.
              </h4>
              <v-textarea
                outlined
                v-model="text"
                rows="6"
                name="input-7-4"
                label="Start writing something..."
                required
              ></v-textarea>

              <v-btn
                class="btn-grad"
                :loading="isLoading"
                @click="submit"
                block
                color="primary"
                >Correct</v-btn
              >
            </div>
          </div>
        </v-col>
        <v-col cols="6">
          <div
            style="border: 1px solid rgb(34, 34, 34)"
            class="tw-m-8 tw-p-8 tw-rounded tw-min-h-[70vh]"
          >
            <!-- <div
              style="border: 1px solid rgb(34, 34, 34)"
              class="tw-px-4 tw-py-2 tw-mb-4 tw-cursor-pointer tw-rounded tw-inline-block"
            >
              Copy
            </div> -->
            <v-btn @click="copyToClipboard" class="btn-grad-2">{{
              copyText
            }}</v-btn>
            <div
              style="border: 1px solid rgb(34, 34, 34)"
              class="tw-rounded tw-w-full tw-h-full tw-p-4 tw-bg-[#0A0A0A]"
            >
              <div
                v-if="!corrected_text"
                class="tw-flex tw-h-[50vh] tw-justify-center tw-items-center"
              >
                <div class="tw-flex tw-flex-col">
                  <img class="tw-h-20" src="../assets/binoculars.svg" alt="" />
                  <div class="tw-my-4">
                    <p class="tw-text-center">Nothing to check yet!</p>
                    <p class="tw-text-center">
                      Start writing or pasting something in the textarea to
                      begin.
                    </p>
                  </div>
                </div>
              </div>
              <div v-else class="tw-min-h-[50vh]">
                {{ corrected_text }}
              </div>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "home-page",
  data() {
    return {
      text: "",
      corrected_text: "",
      isLoading: false,
      copyText: "Copy",
    };
  },
  methods: {
    copyToClipboard() {
      if (this.corrected_text) {
        navigator.clipboard.writeText(this.corrected_text);
      }
    },
    submit() {
      if (this.text) {
        this.isLoading = true;
        axios
          .post("http://127.0.0.1:5000/correct", {
            text: this.text,
          })
          .then((resp) => {
            this.corrected_text = resp.data.corrected_text;
          })
          .catch((err) => console.log(err))
          .finally(() => {
            this.isLoading = false;
          });
      }
    },
  },
};
</script>

<style scoped>
.btn-grad {
  background-image: linear-gradient(
    to right,
    #feac5e 0%,
    #c779d0 51%,
    #feac5e 100%
  );
  margin: 10px;
  padding: 15px 45px;
  text-align: center;
  text-transform: uppercase;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  /* box-shadow: 0 0 20px #eee; */
  border-radius: 10px;
  display: block;
}

.btn-grad:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
}

.btn-grad-2 {
  margin: 0;
  background-image: linear-gradient(
    to right,
    #c084f9 0%,
    #fbcb8c 51%,
    #f992ae 100%
  );
  margin: 10px;
  padding: 15px 45px;
  text-align: center;
  text-transform: uppercase;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  /* box-shadow: 0 0 20px #eee; */
  border-radius: 10px;
  display: block;
}

.btn-grad-2:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
}
.boujee-text {
  --bg-size: 400%;
  --color-one: hsl(15 90% 55%);
  --color-two: hsl(40 95% 55%);
  font-size: clamp(1.6rem, 2vmin, 8rem);
  background: linear-gradient(90deg, #c084f9, #fbcb8c, #f992ae, #c084f9) 0 0 /
    var(--bg-size) 100%;
  color: transparent;
  background-clip: text;
}
@media (prefers-reduced-motion: no-preference) {
  .boujee-text {
    animation: move-bg 10s linear infinite;
  }
  @keyframes move-bg {
    to {
      background-position: var(--bg-size) 0;
    }
  }
}
@media screen and (max-width: 600px) {
  .boujee-text {
    font-size: clamp(1.5rem, 5vmin, 3rem);
  }
}
</style>
