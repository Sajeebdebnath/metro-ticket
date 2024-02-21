<script setup>
import { ref, computed } from "vue";
import { stations, fareMatrix } from "./data/mrt";

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
  <div>
    <div class="check-pass-user">
      <label for="">If Have MRT/Rapid Pass ?</label>
      <input type="checkbox" name="passUser" v-model="passUser" />
    </div>
    <label for="origin">Origin:</label>
    <select v-model="origin" @change="calculateTicketFee">
      <option v-for="station in filteredOrigins" :key="station">
        {{ station }}
      </option>
    </select>

    <label for="destination">Destination:</label>
    <select v-model="destination" @change="calculateTicketFee">
      <option v-for="station in filteredDestinations" :key="station">
        {{ station }}
      </option>
    </select>

    <div v-if="updatedTicketFee !== null">
      Ticket Fee: ${{ updatedTicketFee }}
    </div>
  </div>
</template>

<style scoped></style>
