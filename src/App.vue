<template>
  <div id="app">
    <SchoolsTable />
    <h4>Select a start and end date to show schools scheduled events</h4>
    <div class="datepicker">
      <div>
        <h4>Select start date</h4>      
        <Datepicker v-model="startDate"> </Datepicker>
      </div>
      <div>
        <h4>Select end date</h4>      
        <Datepicker v-model="endDate" v-on:selected="getSchoolsByDate"> </Datepicker>
      </div>
    </div>

    <div class="table-fluid" v-if="schools">
            <table>
                <thead>
                    <!-- <tr v-for="heading in headings" :key="heading.key"> -->
                     <tr>
                        <th>Key</th>
                        <th>Headline</th>
                        <th>SubHeadline</th>
                        <th>Start Time</th>                        
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="school in schools" :key="school.key">
                        <!-- <td v-for="heading in headings" :key=heading.key></td> -->
                        <td>{{ school.key }} </td>
                        <td>{{ school.headline }}</td>
                        <td>{{ school.subheadline }}</td>
                        <td>{{ school.date }}</td>
                    </tr>
                </tbody>
            </table>
        </div>            
  </div>
</template>

<script>
import SchoolsTable from './components/SchoolsTable.vue'
import Datepicker from 'vuejs-datepicker'

export default {
  name: 'App',
  components: {
    SchoolsTable,
    Datepicker    
  },
  data: function() {
    return {
      startDate: new Date(),
      endDate: new Date(),
      schools: [],
      schoolsUrl: "https://challenge.nfhsnetwork.com/v2/search/events/upcoming?state_association_key=18bad24aaa&card=true&size=50&start=0",
    }
  },
  methods: {
    getSchoolsByDate() {
      this.schools = [];
      this.$http.get(`${this.schoolsUrl}&from=2021-02-20T21:00:00.000Z&to=2021-04-15T21:00:00.000Z`)
      .then((schoolsResults) => {
          this.schools = schoolsResults.data.items;
          const result = this.schools.filter((dateObj) => {
            // console.log('this.startDate :>> ', this.startDate);
            // console.log('this.endDate :>> ', this.endDate);
            return dateObj.date >= this.startDate && dateObj.date <= this.endDate;
          });
          console.log('result from getSchoolsByDate :>> ', result);
      });
    }
  }
}
</script>
// 
<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  h4 {
    text-align: left;
  }
  .datepicker {
    display: flex;
    justify-content: space-evenly;
  }

    .table-fluid {
        overflow-x: auto;
        margin-top: 30px;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 30px 0;
    }
    table,
    th,
    td {
        border: 1px solid black;
    }
    thead {
        background-color: #1c87c9;
        color: #ffffff;
    }
    th {
        text-align: center;
        height: 50px;
    }
    tbody tr:nth-child(odd) {
        background: #ffffff;
    }
    tbody tr:nth-child(even) {
        background: #f4f4f4;
    }
    tr:hover {
        background-color: #cdcdcd;
    }
</style>
