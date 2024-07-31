<script>
import axios from 'axios';

export default{
  data(){
      return {
          store: [],
          filteredResults: [],
          searchQuery: ''
      }
    },
  created() {
      axios.get('https://olympics.com/OG2024/data/CIS_MedalNOCs~lang=ITA~comp=OG2024.json').then(res => {

          this.store = res.data.medalNOC
          this.filteredResults = res.data.medalNOC.filter(item => item.sport === 'GLO' && item.gender === 'TOT').sort((a, b) => a.rank - b.rank)
          console.log(this.filteredResults)

      }).catch(err => {
        console.log(err)
      });
    },
  methods: {
      applyFilter() {
        const formattedQuery = this.searchQuery.charAt(0).toUpperCase() + this.searchQuery.slice(1).toLowerCase();
        this.filteredResults = this.store.filter(item => 
          item.sport === 'GLO' && 
          item.gender === 'TOT' &&
          item.organisation.description.includes(formattedQuery)
        ).sort((a, b) => a.rank - b.rank);
      }
    }
}
</script>

<template>
    <div class="px-5">
        <div class="mb-4 d-flex justify-content-end">
            <input 
              v-model="searchQuery" 
              @input="applyFilter" 
              placeholder="Enter country" 
              type="text" 
            />
        </div>

        <div>
            <table class="table text-center">
                <thead>
                    <tr>
                        <th scope="col">Posizione</th>
                        <th scope="col">Nazionalità</th>
                        <th scope="col">Oro</th>
                        <th scope="col">Argento</th>
                        <th scope="col">Bronzo</th>
                        <th scope="col">Totale</th>
                    </tr>
                </thead>
                <tbody v-for="medalData in filteredResults">
                    <tr>
                        <th scope="col">{{medalData.rank}}</th>
                        <th scope="col">{{medalData.organisation.description}}</th>
                        <th scope="col">{{medalData.gold}}</th>
                        <th scope="col">{{medalData.silver}}</th>
                        <th scope="col">{{medalData.bronze}}</th>
                        <th scope="col">{{medalData.total}}</th>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<style>

</style>
