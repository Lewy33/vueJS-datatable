<template>
    <div class="liste">
        <table>
            <thead>
                <tr>
                    <th>First name</th>
                    <th>Last name</th>
                    <th>Titre</th>
                    <th>Description</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="inter in interventions">
                    <td>{{inter.first_name}}</td>
                    <td>{{inter.last_name}}</td>
                    <td>{{inter.titre}}</td>
                    <td>{{inter.description}}</td>
                </tr>
            </tbody>
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
                interventions:[],
                columns: []
            }
        },
        mounted() {
            axios.get(`https://raw.githubusercontent.com/mdubourg001/datatable_vuejs/master/src/assets/REDUCED_DATA.json`)
                .then(response => {
                    // JSON responses are automatically parsed.
                    this.interventions = response.data;
                    console.log(this.interventions)
                })
                .catch(e => {
                    this.errors.push(e)
                })
        },
    }
</script>

<style>
    body {
        font-family: Arial, sans-serif;
        font-size: 14px;
        color: #444;
    }

    table {
        border: 2px solid #3b45b9;
        border-radius: 3px;
        background-color: #fff;
    }

    th {
        background-color: #3b45b9;
        color: rgba(255,255,255,1);
        cursor: pointer;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }

    td {
        background-color: #f9f9f9;
    }

    th, td {
        min-width: 120px;
        padding: 10px 20px;
    }

    th.active {
        color: #fff;
    }

    th.active .arrow {
        opacity: 1;
    }

    .arrow {
        display: inline-block;
        vertical-align: middle;
        width: 0;
        height: 0;
        margin-left: 5px;
        opacity: 0.66;
    }

    .arrow.asc {
        border-left: 4px solid transparent;
        border-right: 4px solid transparent;
        border-bottom: 4px solid #fff;
    }

    .arrow.dsc {
        border-left: 4px solid transparent;
        border-right: 4px solid transparent;
        border-top: 4px solid #fff;
    }


</style>