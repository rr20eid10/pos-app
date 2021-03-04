<template class="main">
    <div>     
        <h3>Select an option in the below input fields to show schools.</h3>    
        <form v-on:submit.prevent="getSchools" class="form-inline">
            <div class="form-box">
            <label>Search Term:</label>
                <select v-model="searchTerm">
                    <option value="18bad24aaa">Georgia</option>
                    <option value="542bc38f95">Texas</option>
                </select>
            </div>

            <div class="form-box">
            <label>Search By:  </label>
                <select v-model="searchBy">
                    <option 
                        v-for="option in searchByOptions" 
                        :key="option.index"> 
                            {{ option }}
                    </option>
                </select>
            </div>
            <div class="form-box">
                <!-- <button v-bind:class="[checkSelectInputs ? cursorNotAllowed : '', cursorAllowed]" :disabled="checkSelectInputs">Get Schools</button> -->
                <button type="submit">Get Schools</button>
            </div>               
        </form>

        <div v-if="schools">
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


export default {
  name: 'SchoolsTable',
  data: function() {
      return {
            // searchByOptions are the parameters to search by shown in the API and how I can build my URL GET request
            searchByOptions: ["state_association_key"],
            
            // whatever the user types ingets concatenated on the URL as ${this.searchTerm} after the "=" sign
            searchTerm: "",

            // the item the user selected in the dropdown this gets concatenated on the URL after the "?" ${this.searchBy} per API
            searchBy: "",

            schools: [],
            schoolsUrl: "https://challenge.nfhsnetwork.com/v2/search/events/upcoming", 
            cursorNotAllowed: "bg-gray",
            cursorAllowed: "shadow"
      }
  },
  methods: {
      getSchools: function() {
          this.schools = [];
          this.$http.get(`${this.schoolsUrl}?${this.searchBy}=${this.searchTerm}&card=true&size=50&start=0`)
          .then((schoolsResults) => {
              this.schools = schoolsResults.data.items
              });
      }
  },
    computed: {
    "headings": function headings() {
      if (this.schools.length == 0) {
        return [];
      }
      return Object.keys(this.schools[0])
    } 
    //Validate the form fields so that the user doesn't submit an empty form
    // checkSelectInputs() {
    //     if(this.searchBy === "" || this.searchTerm === ""){
    //         return true;
    //     }
    // }
  }

  
  
}
</script>

<style >
    /*********************
        Default
    *********************/

    .main {
        margin: 1rem auto;
        width: 80%;
        max-width: 1000px;
    }

    h3,
    h4 {
        color:#135d86;
        text-align: left;
    }

    /*********************
        Form
    *********************/
    select, option, input, label, button{
        padding: 0.2rem 1rem;
        font-size: 1.2rem;
        color: inherit;
    }
    .form-box{
        padding: 0.5rem 0;
        display: flex;
        flex-direction: column;
    }

    .form-box input,
    .form-box select{
        flex-basis: 100%;
    }

    .form-box button{
        background-color: #1c87c9;
        color: #fff; 
        
    }

    .form-box button:hover {
        background-color: royalblue;
    }

    .table-fluid {
        overflow-x: auto;
    }
    table {
        width: 100%;
        border-collapse: collapse;
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

    .bg-gray {
        background-color: gray;
    }
    .shadow {
        background-color: rosybrown;
    }


    /* Add responsiveness - display the form controls vertically instead of horizontally on screens that are less than 800px wide */
    /*******************************
        Media Queries
    *******************************/
    @media all and (min-width: 500px){

        .form-box label{
            display: block;
        }
    
    }

    @media all and (min-width: 600px){
        .form-box{
            flex-direction: row;
        }
        .form-box label{
            flex-basis: 9rem;  
            flex-grow: 0;
            flex-shrink: 0;
        }
        .form-box input,
        .form-box select{
            flex-basis: 0;
            flex-grow: 1;
            flex-shrink: 1;
        }
        .form-box button{
            margin-left: 11rem;
        }
    }

</style>