<template>
  <v-app>    
<v-card
    class="mx-auto"
    max-width="auto"
    outlined
  >
    <v-list-item three-line>
      <v-list-item-content>
        
        <v-list-item-title class="headline mb-1 text-center">Indian Cases : {{ indiaTotalCase }}</v-list-item-title>
        <v-list-item-title class="headline mb-1 text-center">Indian Deaths : {{ indianDeaths }}</v-list-item-title>
        <v-list-item-title class="headline mb-1 text-center">Recoveries : {{ indianDischarged }}</v-list-item-title>
        
        </v-list-item-content>
    </v-list-item>

  </v-card>


<!----------------------------------------------------------------------->

<br><br>
 
 <div class="text-center">
    <v-btn rounded color="primary" dark @click="getdata">Refresh</v-btn>
  </div>
  <br><br><br>

  <v-divider></v-divider>    
  
      <v-data-table
        :items="users"
        :headers="headers"
        class="elevation-1"
        :dark="true"
      >
    
    <template v-slot:item.deaths="{ item }">
      <v-chip :color="getColor(item.deaths)" dark>{{ item.deaths }}</v-chip>
    </template>
  </v-data-table>
  </v-app>
  
</template>

<script>


export default {

 data () {
      return {
        users: [],
        indiaTotalCase: null,
        indianDeaths: null,
        indianDischarged: null,
        headers: [
          { text: 'State', align: 'start', sortable: false, value: 'loc'},
          { text: 'Cases', value: 'totalConfirmed' },
          { text: 'Deaths', value: 'deaths' },
          { text: 'Discharged', value: 'discharged' }
          
        ],
        
      }
    },
    methods: {
      getColor (deaths) {
        if (deaths > 100) return 'red'
        else if (deaths > 50) return 'orange'
        else return 'green'
      },
       getdata() {
        this.$http.get('https://api.rootnet.in/covid19-in/stats/latest')
          .then(res =>{
              const data = res.body.data;
                this.indiaTotalCase = data.summary.total;
                this.indianDeaths = data.summary.deaths;
                this.indianDischarged = data.summary.discharged;
                const regionalData = data.regional;
                this.users = regionalData;
    });
    },
    },
    mounted(){
      this.getdata();
      console.log("call finished");
    },
};
</script>
