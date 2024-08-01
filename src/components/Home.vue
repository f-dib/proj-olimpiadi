<script>
import axios from 'axios';

export default{
  data(){
      return {
          store: [],
          filteredResults: [],
          searchQuery: '',
          flags: []
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
      },
      olympicFlag(currentFlag) {
        let flag = currentFlag.toLowerCase()

        return `src/assets/done/${flag}.svg`;
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
              placeholder="Cerca..." 
              type="text"
              class="rounded-4 ps-2 border-secondary" 
            />
        </div>

        <table class="table text-center">
            <thead>
                <tr>
                    <th scope="col">Posizione</th>
                    <th scope="col"></th>
                    <th scope="col">Nazionalità</th>
                    <th scope="col">Oro</th>
                    <th scope="col">Argento</th>
                    <th scope="col">Bronzo</th>
                    <th scope="col">Totale</th>
                </tr>
            </thead>
            <tbody v-for="medalData in filteredResults">
                <tr class="fw-bold">
                    <td scope="col">{{medalData.rank}}</td>
                    <td scope="col" class="align-middle flag-cell">
                      <div class="flags d-flex justify-content-center align-items-center">
                        <img :src="olympicFlag(medalData.organisation.code)" class="img-fluid" alt="Olympic Logo" />
                      </div>
                    </td>
                    <td scope="col">{{medalData.organisation.description}}</td>
                    <td scope="col">{{medalData.gold}}</td>
                    <td scope="col">{{medalData.silver}}</td>
                    <td scope="col">{{medalData.bronze}}</td>
                    <td scope="col">{{medalData.total}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style>

</style>
