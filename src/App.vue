<script setup>
    import { ref, reactive } from 'vue'
    const countries = reactive([
        { name: "Bangladesh", capital: "Dhaka" },
        { name: "Bhutan", capital: "Thimpu" },
    ])

    function getAllCountries (list = 'All') {
        countries.length=0
        fetch('https://restcountries.com/v3.1/all?fields=name,capital')
            .then(response => response.json())
            .then(data => {
                if ('All' == list) {
                    data.forEach(country => {
                        countries.push({
                            name: country.name.common,
                            capital: country.capital[0]
                        })
                    })
                } else {
                    data.filter(c=>c.name.common.startsWith(list)).forEach(country => {
                        countries.push({
                            name: country.name.common,
                            capital: country.capital[0]
                        })
                    })
                }
            })
        }
</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <h2 class="mt-3">Country List</h2>
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
            <div class="col-md-12">
                
            </div>
        </div>
    </div>
</template>

<style scoped>
    
</style>