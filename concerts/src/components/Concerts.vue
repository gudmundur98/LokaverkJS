<template>
  <div id="concerts" class="row">
    <b-col cols=12>
      <b-form>
        <b-form-group label="Leit" class="mr-3">
          <b-input v-model="nameSearchString"></b-input>
        </b-form-group>
        <b-form-group label="Dagsetning" class="mr-3">
          <b-input type="date" v-model="startDate"></b-input>&nbsp;
          <b-input type="date" v-model="endDate"></b-input>
        </b-form-group>
        <b-form-group label="Staðsetning:">
          <b-form-checkbox-group id="checkboxes2" name="flavour2" v-model="venuesSelected">
            <b-form-checkbox v-for="venue in venues" v-bind:key="venue" :value="venue">{{ venue }}</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
      </b-form>
    </b-col>
    <b-col class="d-flex flex-row flex-wrap justify-content-between">
      <div v-for="concert in filteredConcerts" v-bind:key="concert.eventDateName+concert.dateOfShow" class="d-flex mt-4">
        <b-card :title="concert.eventDateName"
                :img-src="concert.imageSource"
                :img-alt="concert.eventDateName"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2">
          <p class="card-text">
            {{ concert.eventHallName }} - {{ concert.userGroupName }}<br>
            {{ moment(concert.dateOfShow).format("dddd Do MMMM HH:mm") }}
          </p>
          <b-button href="https://midi.is" variant="primary">Kaupa miða!</b-button>
        </b-card>
      </div>
    </b-col>
  </div>
</template>

<script>
import moment from 'moment';
import axios from 'axios';

moment.locale('is');

export default {
  name: 'Concerts',
  data() {
    return {
      moment,
      concerts: [],
      venuesSelected: [],
      nameSearchString: "",
      startDate: moment().format('YYYY-MM-DD'),
      endDate: moment().add(1, 'month').format('YYYY-MM-DD')
    };
  },
  computed: {
    filteredConcerts(){
      let concerts = this.concerts;
      concerts = concerts.filter(x => {
        return x.eventDateName.includes(this.nameSearchString);
      });
      
      concerts = concerts.filter(x => {
        return this.venuesSelected.length === 0 || this.venuesSelected.includes(x.eventHallName);
      })

      concerts = concerts.filter(x => {
        return moment(x.dateOfShow).isBetween(moment(this.startDate), moment(this.endDate));
      })

      return concerts;
    },
    venues() {
      let venues = this.concerts.map(x => x.eventHallName);

      // var myArray = ['a', 1, 'a', 2, '1'];
      let unique = venues.filter((v, i, a) => a.indexOf(v) === i);

      return unique;
    }
  },
  mounted(){
    axios.get('https://apis.is/concerts')
    .then(response => {
      console.log(response);
      this.concerts = response.data.results;
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
