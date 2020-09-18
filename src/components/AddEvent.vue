<template>
    <div class="homeText">
        Syötä tapahtuma
        <form @submit.prevent="handleSubmit">
            <div class="formSlot">
                <label class="left">Tapahtuman nimi</label>
                <br>
                <input class="right"
                       type="text"
                       ref="name"
                       :class="{ 'has-error': submitting && invalidEventName}"
                       v-model="eventName"
                       @focus="clearStatus"
                       @keypress="clearStatus"
                       placeholder="esim. Kalaretki"
                />
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuman sijainti</label>
                <br>
                <input class="right"
                       type="text"
                       :class="{ 'has-error': submitting && invalidEventLocation}"
                       v-model="eventLocation"
                       @focus="clearStatus"
                       placeholder="esim. Järvenpää"
                />
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuma alkaa </label>
                <br>
                <input class="right"
                       type="date"
                       v-model="eventStart"
                />
            </div>
            <div class="formSlot">
                <label class="left">Tapahtuma loppuu</label>
                <br>
                <input class="right"
                       type="date"
                       v-model="eventEnd"
                />
            </div>
            <p v-if="error && submitting" class="error-message">
                Ole hyvä ja täytä kaikki kentät!
            </p>
            <p v-if="success" class="success-message">Tiedot lähetetty!</p>
            <button>Tallenna</button>

        </form>
    </div>
</template>

<script>

  export default {
    name: 'AddEvent',
    methods: {
      handleSubmit() {
        this.submitting = true;
        this.clearStatus();

        if (this.invalidEventName || this.invalidEventLocation || this.invalidEventStart || this.invalidEventEnd) {
          console.log("joku tyhjä");
          this.error = true;
          return;
        }

        let newEvent = {
          name: capitalize(this.eventName),
          location: capitalize(this.eventLocation),
          dateStart: fixDates(this.eventStart),
          dateEnd: fixDates(this.eventEnd),
          id: this.items.length + 1,
        };
        console.log(newEvent);
        this.method(newEvent);
        this.eventName = '';
        this.eventLocation = '';
        this.eventStart = '';
        this.eventEnd = '';
        this.error = false;
        this.$refs.name.focus();
        this.success = true;
        this.submitting = false;
      },
      clearStatus() {
        this.success = false;
        this.error = false;
      },
    },
    data() {
      return {
        submitting: false,
        error: false,
        success: false,
        eventName: '',
        eventLocation: '',
        eventStart: '',
        eventEnd: '',
        newEvent: {},
      }
    },
    props: {
      items: Array,
      method: { type: Function }
    },
    computed: {
      invalidEventName() {
        return this.eventName.trim() === "" || this.eventName.length < 5;
      },
      invalidEventLocation() {
        return this.eventLocation.trim() === "" || this.eventLocation.length < 3;
      },
      invalidEventStart() {
        return this.eventStart === '' || stringToDate(this.eventStart) < Date.now();
      },
      invalidEventEnd() {
        return this.eventEnd === '' || stringToDate(this.eventEnd) < stringToDate(this.eventStart) ;
      }
    }
  };

  const capitalize = (value) => {
    return value.toLowerCase()
    .split(/ /)
    .map((s) => s.charAt(0).toUpperCase() + s.substring(1))
    .join(' ');
  };

  const stringToDate = (s) => {
    return new Date(s);
  };

  const fixDates = (string) => {
    let from = string.split("-");
    return `${from[2]}.${from[1]}.${from[0]}`;
  }

</script>

<style scoped>
    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: darkred;
    }

    .success-message {
        color: seagreen;
    }

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
