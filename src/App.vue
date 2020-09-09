<template>
    <b-container id="app">
        <b-row>
            <b-col>
                <navbar/>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
                <router-view :items="events" :method="updateEventData"/>
            </b-col>
        </b-row>
    </b-container>

</template>

<script>
  import Navbar from '@/components/Navbar';
  import axios from 'axios';
  import {mapActions, mapGetters} from 'vuex';

  export default {
    name: 'App',
    components: {
      Navbar,
    },
    computed: {
      ...mapGetters([
        'events'
      ])
    },
    mounted() {
      this.getEventData()
    },
    methods: {
      ...mapActions([
        'fillEvents'
      ]),
      getEventData() {
        const promise = axios.get('http://localhost:8081/events');
        promise.then(response => {
          this.fillEvents(response.data);
        })
      },
      updateEventData(newEvent) {
        axios
        .post('http://localhost:8081/events', newEvent)
        .then(response => {
          if (response.status === 201) {
            this.fillEvents(this.events.concat(newEvent));
          } else {
            alert("Data not saved!");
          }
        });
      }
    },
  };

</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

</style>
