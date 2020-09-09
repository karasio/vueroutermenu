<template>
    <div class="homeText">
        Syötä tapahtuma
        <form @submit.prevent="handleSubmit">
            <div class="formSlot">
                <label class="left">Tapahtuman nimi</label>
                <br>
                <input class="right"
                       type="text"
                       v-model="eventName"
                       name="name"
                       placeholder="esim. Kalaretki">
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuman sijainti</label>
                <br>
                <input class="right"
                       type="text"
                       v-model="eventLocation"
                       name="location"
                       placeholder="esim. Järvenpää">
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuma alkaa </label>
                <br>
                <input class="right"
                       type="date"
                       v-model="eventStart"
                       name="timeStart" >
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuma loppuu</label>
                <br>
                <input class="right"
                       type="date"
                       v-model="eventEnd"
                       name="timeEnd">
            </div>
            <button>Tallenna</button>

        </form>
    </div>
</template>

<script>

  export default {
    name: 'AddEvent',
    methods: {
      handleSubmit() {
        let newEvent = {
          name: capitalize(this.eventName),
          location: capitalize(this.eventLocation),
          dateStart: fixDates(this.eventStart),
          dateEnd: fixDates(this.eventEnd),
          id: this.items.length + 1,
        };
        console.log(newEvent);
        this.method(newEvent);
      },
    },
    data() {
      return {
        eventName: null,
        eventLocation: null,
        eventStart: null,
        eventEnd: null,
        newEvent: {},
      }
    },
    props: {
      items: Array,
      method: { type: Function }
    }
  };

  const capitalize = (value) => {
    return value.toLowerCase()
    .split(/ /)
    .map((s) => s.charAt(0).toUpperCase() + s.substring(1))
    .join(' ');
  };

  const fixDates = (string) => {
    let from = string.split("-");
    return `${from[2]}.${from[1]}.${from[0]}`;
  }

</script>

<style scoped>


    .formSlot {
        margin: 5px;
    }

    form {
        font-size: initial;
    }

    .homeText {
        font-size: 35px;
        color: midnightblue;
        text-align: center;
        position: relative;
        top: 30px;
        text-shadow: 2px 2px 2px gray;
    }
</style>
