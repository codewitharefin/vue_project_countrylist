<script setup>
    import { ref, reactive } from 'vue'
    
    // const countries = reactive([
    //     { flag: "", name: "Bangladesh", capital: "Dhaka" },
    //     { flag: "", name: "Bhutan", capital: "Thimpu" },
    // ])

    const countries = reactive([])

    getAllCountries()

    function getAllCountries (list = 'All') {
        countries.length=0
        fetch('https://restcountries.com/v3.1/all?fields=name,capital,flag,flags,population,timezones,startOfWeek,currencies,tld')
            .then(response => response.json())
            .then(data => {
                if ('All' == list) {
                    data.forEach(country => {
                        countries.push({
                            name: country.name.common,
                            capital: country.capital[0],
                            flag: country.flag,
                            flags: country.flags.svg,
                            population: country.population,
                            timezones: country.timezones[0],
                            startOfWeek: country.startOfWeek,
                            currencies: country.currencies,
                            tld: country.tld[0]
                        })
                    })
                } else {
                    data.filter(c => c.name.common.startsWith(list)).forEach(country => {
                        countries.push({
                            name: country.name.common,
                            capital: country.capital[0],
                            flag: country.flag,
                            flags: country.flags.svg,
                            population: country.population,
                            timezones: country.timezones[0],
                            startOfWeek: country.startOfWeek,
                            currencies: country.currencies,
                            tld: country.tld[0]
                        })
                    })
                }
                countries.sort((a, b) => a.name.localeCompare(b.name));
            })
        }
</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <h3 class="mt-3 text-center">Country List</h3>
                <div class="d-flex justify-content-center mb-3">
                    <button class="btn btn-primary btn-sm m-1" @click="getAllCountries()">
                        List of Countries
                    </button>
                    <button class="btn btn-primary btn-sm m-1" @click="getAllCountries('A')">
                        A
                    </button>
                    <button class="btn btn-primary btn-sm m-1" @click="getAllCountries('B')">
                        B
                    </button>
                    <button class="btn btn-primary btn-sm m-1" @click="getAllCountries('C')">
                        C
                    </button>
                </div>
            </div>
            <div class="col-md-12">
                <div class="table-responsive">
                    <table class="table table-bordered border-secondary">
                        <thead>
                            <tr>
                                <th>Flag</th>
                                <th>Flag (svg)</th>
                                <th>Country</th>
                                <th>Capital</th>
                                <th>Polulation</th>
                                <th>Timezone</th>
                                <th>Week Start</th>
                                <th>Currencies</th>
                                <th>TLD</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="country in countries" :key="country.name">
                                <td>{{ country.flag }}</td>
                                <td><img :src="country.flags" alt="" style="width:50px;"></td>
                                <td>{{ country.name }}</td>
                                <td>{{ country.capital }}</td>
                                <td>{{ country.population.toLocaleString() }}</td>
                                <td>{{ country.timezones }}</td>
                                <td>{{ country.startOfWeek }}</td>
                                <td><pre>{{ country.currencies }}</pre></td>
                                <td>{{ country.tld }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    
</style>