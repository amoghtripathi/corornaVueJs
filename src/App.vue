<template>
  <v-app>
    
    <v-card width="600"  class="mx-auto mt-5">
      <v-card-title>
      <h1 class="display-1">
        Login
      </h1>
      </v-card-title>
      <v-card-text>
        <v-form>
          <v-text-field label="Username"
          prepend-icon="mdi-account-circle"
          />
          <v-text-field type="password" label="Password"
          prepend-icon="mdi-lock"
          append-icon="mdi-eye-off"
          />
        </v-form>
      </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-btn color="success">Register</v-btn>
        <v-spacer></v-spacer>
        <v-btn color="info">LogIn</v-btn>
      </v-card-actions>
    </v-card>
<v-divider></v-divider>

<span>Total India Cases : {{indiaTotalCase}}</span>

<v-divider></v-divider>

<span>Maharastra State Cases : {{users[19].confirmedCasesIndian}}</span>



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

  <!-------
    <template slot="items" slot-scope="props">

        <td v-for="(header, index) in headers" :key="index">{{ props.item[header.value] }}</td>   ----------->

  <!-------
  <template v-slot:items="props">
    <td>{{ props.item.loc }}</td>
    <td>{{ props.item.totalConfirmed }}</td>
    <td>{{ props.item.deaths }}</td>
    <td>{{ props.item.discharged }}</td>  
  </template>
  -------->
  
    
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
        .then(response =>{
          return response.json();
        })
        .then(res =>{
          const data = res.data;
          this.indiaTotalCase = data.summary.total;
          const regionalData = data.regional;
       
          for(let index in regionalData){
            this.users[index] = regionalData[index];
          }
         
        });
    }
    },
    mounted(){
      this.getdata();
      console.log("call finished");
    },
};
</script>
