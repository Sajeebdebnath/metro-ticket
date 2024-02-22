<script setup>
import { ref, computed } from "vue";
import { stations, fareMatrix } from "@/data/mrt";
import { convertToBengaliNum } from "@/composables/convert";
import logo from "@/assets/images/logo.png";
import heroBg from "@/assets/images/hero-bg.jpg";
import ticketBg from "@/assets/images/ticketbox-bg.jpg";

const origin = ref("");
const destination = ref("");
const passUser = ref(false);
const ticketFee = ref(null);

const filteredDestinations = computed(() => {
  if (origin.value) {
    return stations.filter((station) => station !== origin.value);
  } else {
    return stations;
  }
});

const filteredOrigins = computed(() => {
  if (destination.value) {
    return stations.filter((station) => station !== destination.value);
  } else {
    return stations;
  }
});

const calculateTicketFee = () => {
  if (origin.value && destination.value) {
    ticketFee.value = fareMatrix[origin.value][destination.value];
  }
};

const updatedTicketFee = computed(() => {
  if (passUser.value && ticketFee.value !== null) {
    return ticketFee.value * 0.9;
  } else {
    return ticketFee.value;
  }
});
</script>

<template>
  <header class="header-area">
    <div class="container">
      <div class="header-logo">
        <a href=""><img :src="logo" alt="Mictro Ticket" width="100" /></a>
      </div>
    </div>
  </header>
  <section>
    <div
      class="hero-area"
      :style="{ 'background-image': 'url(' + heroBg + ')' }"
    >
      <div class="container h-100">
        <div class="row algin-center h-100">
          <div class="col-lg-7 col-md-5">
            <div class="hero-banner-info">
              <h3 class="subtitle">ঢাকা মেট্রোরেল</h3>
              <h2 class="title">
                MRT <span><b>Line</b> <b>6</b></span>
              </h2>
            </div>
          </div>
          <div class="col-lg-5 col-md-7">
            <div
              class="metro-ticket-form"
              :style="{ 'background-image': 'url(' + ticketBg + ')' }"
            >
              <form name="form">
                <div class="form-group">
                  <label for="">এমআরটি / রেপিড পাস আছে ?</label> -
                  <input type="checkbox" name="passUser" v-model="passUser" />
                </div>
                <div class="form-group">
                  <label for="origin">মূল স্টেশন :</label>
                  <select
                    class="form-control"
                    v-model="origin"
                    @change="calculateTicketFee"
                  >
                    <option value="" disabled>মূল স্টেশন নির্বাচন করুন</option>
                    <option v-for="station in filteredOrigins" :key="station">
                      {{ station }}
                    </option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="destination">গন্তব্য স্টেশন :</label>
                  <select
                    class="form-control"
                    v-model="destination"
                    @change="calculateTicketFee"
                  >
                    <option value="" disabled>
                      গন্তব্য স্টেশন নির্বাচন করুন
                    </option>
                    <option
                      v-for="station in filteredDestinations"
                      :key="station"
                    >
                      {{ station }}
                    </option>
                  </select>
                </div>
                <div class="ticket-info" v-if="updatedTicketFee !== null">
                  ভাড়া : {{ convertToBengaliNum(updatedTicketFee) }} টাকা মাত্র
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
