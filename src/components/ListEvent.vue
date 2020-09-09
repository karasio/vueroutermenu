<template>
    <div>
<!--        <p class="homeText">-->
<!--            Listaa tapahtumia-->
<!--        </p>-->
        <div id="query-form">
<!--            <form @submit.prevent="handleSubmit">-->
<!--                <button>Hae</button>-->
                <table id="event-table" v-bind:items="items" >
                    <thead>
                        <tr>
                            <td>Mitä?</td>
                            <td>Missä?</td>
                            <td>Koska?</td>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="event in items" :key="event.id">
                            <td> {{ event.name }}</td>
                            <td> {{ event.location }}</td>
                            <td> {{ event.dateStart === event.dateEnd
                                ? event.dateStart
                                : event.dateStart + " - " + event.dateEnd}}</td>
                        </tr>
                    </tbody>
                </table>
<!--            </form>-->
        </div>
    </div>
</template>

<script>



  export default {
    name: 'ListEvent',
    data() {
      return {

      }
    },
    mounted() {

    },
    computed: {                 // esim esikäsittele lomake (validoi)

    },
    // methods: {
    //   async handleSubmit() {
    //     console.log("Nappia painettu");
    //   }
    // },
    props: {
      items: Array,
    },

  };
</script>

<style scoped>
    thead {
        font-weight: bold;
        font-size: larger;
    }
    td {
        border-width: 1px;
        border-style: solid;
        border: coral;
    }
    .homeText {
        font-size: 35px;
        color: red;
        text-align: center;
        position: relative;
        top: 30px;
        text-shadow: 2px 2px 2px gray;
    }
</style>
