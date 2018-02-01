<template>
    <div class="liste">
        <table>
            <thead>
                <tr>
                    <th v-for="">

                    </th>
                </tr>
            </thead>
            <tr>
                <td v-for="inter in interventions">{{inter}}</td>
            </tr>
        </table>
    </div>
</template>

<script>
    import * as axios from "axios";

    export default {
        name: 'Liste',
        props: {
            data: Array,
            columns: Array,
            filterKey: String
        },
        data(){
            return {
                interventions:[]
            }
        },
        mounted() {
            axios.get(`https://raw.githubusercontent.com/mdubourg001/datatable_vuejs/master/src/assets/MOCK_DATA.json`)
                .then(response => {
                    // JSON responses are automatically parsed.
                    this.interventions = response.data
                })
                .catch(e => {
                    this.errors.push(e)
                })
        }
    }
</script>

<style>
    td{
        display: inline-block;
    }
</style>