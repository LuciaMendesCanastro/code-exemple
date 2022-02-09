<template>
    <div class="city-search_card">
        <h1 class="city-search_title">City Finder</h1>
            <div class="city-search_align">
                <input
                    class="city-search_input"
                    type="text"
                    name="city-search"
                    placeholder="Recherchez une ville"
                    autocomplete="off"
                    v-model="searchTerms"
                >
                <button
                    class="city-search_btn"
                    @click="sendSearchRequest(searchTerms)"
                >
                    Rechercher
                </button>
            </div>
        <div v-if="results.length > 0">
            <p class="city-search_result_title">Résultats de la recherche</p>
            <ul>
                <li
                    v-for="(result, index) in displayData"
                    :key="index"
                >
                    <span class="city-search_results">{{result.name}} </span>
                    <span class="city-search_results"> {{result.zip_code}}</span>
                </li>
            </ul>
            <el-pagination
                layout="prev, pager, next"
                :total="results.length"
                @current-change="setPage"
                :page-size="pageSize"
                class="pagination-results"
            >
            </el-pagination>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'CitySearch',
    data() {
        return {
            searchTerms: '',
            results: [],
            page: 1,
            pageSize: 5
        }
    },
    computed:{
        displayData() {
            if (!this.results || this.results.length === 0) return [];
            return this.results.slice(this.pageSize * this.page - this.pageSize, this.pageSize * this.page)
        }
    },
    methods: {
        sendSearchRequest (userInput) {
            axios.get(`https://6dd9b2f3c2c444e3bf48107dee582402.instances.fr1.caas.microservices.rest/api/v1.1/city?search=${userInput}`)
            .then(response => {
                console.log(response)
                this.results = response.data.results;
            })
        },
        setPage (val) {
            this.page = val;
        }
    }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@1,300&display=swap');
.city-search_card{
    margin-top:  20em;
    border: 1px solid #DEDEDE;
    padding: 1em 4em 2em;
    border-radius: 10px;
    width: 20em;
    margin-left: auto;
    margin-right: auto;
}
.city-search_title{
    color:  #53ABE4;
    text-align: center;
    font-family: Roboto;

}
.city-search_input{
    display: block;
    margin-bottom:  2em;
    padding: 0.5em 0 0.5em 1em;
    width: 100%;
    border-radius: 8px;
    border:  1px solid #DEDEDE;

}
.city-search_btn{
    background-color: #47BED6;
    border-radius: 10px;
    padding: 0.5em 1em;
    border:  none;
    color: white;
    cursor: pointer;
    margin-left: auto;
    margin-right: auto;
}
.city-search_align{
    text-align: center;
}
.city-search_btn:hover{
    background-color: #2999B0;
}
.city-search_result_title{
    font-family: Roboto;
    margin-top: 3em;
}
.city-search_results{
    font-family: Roboto;
    margin-top: 3em;
}
.pagination-results{
    text-align: center;
    font-family: Roboto;
}
</style>
