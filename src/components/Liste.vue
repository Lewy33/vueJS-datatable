<template>
    <div class="liste">
        <form method="post" action="#">
            <table>
                <thead>
                    <tr>
                        <th v-for="column in columns"  @click="sortByKey(column)" :class="[
                            column == order.by ? 'ordered':'' ,
                            column == order.by && order.order == 'ASC' ? 'ordered-asc':
                                column == order.by && order.order == 'DESC' ? 'ordered-desc':''
                        ]">
                            {{column}}

                        </th>
                        <th @click="deleteInter(index)">
                            Actions
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="inter in dataByKeyAndOrder">
                        <td>{{inter.id}}</td>
                        <td>{{inter.first_name}}</td>
                        <td>{{inter.last_name}}</td>
                        <td>{{inter.titre}}</td>
                        <td>{{inter.description}}</td>
                        <td><input type="checkbox"/></td>
                    </tr>
                </tbody>
            </table>
        </form>
    </div>
</template>

<script>
    import * as axios from "axios";

    export default {
        name: 'Liste',
        props: {
            data: Array,
            filterKey: String
        },
        data(){
            return {
                interventions:[],
                columns: [],
                order: {
                    by: 'id',
                    order: 'ASC'
                }
            }
        },
        computed: {
            dataByKeyAndOrder(){
                var compare = function (filter) {
                    return function (a,b) { //closure
                        var a = a[filter],
                            b = b[filter];

                        if (a < b) {
                            return -1;
                        }else if (a > b) {
                            return 1;
                        } else {
                            return 0;
                        }
                    };
                };

                var filter = compare(this.order.by);
                var data = this.interventions.sort(filter)
                if(this.order.order === 'ASC') {
                    return data
                }else {
                    return data.reverse()
                }
            }

        },
        methods: {
            fetchData(){
                axios.get(`https://raw.githubusercontent.com/mdubourg001/datatable_vuejs/master/src/assets/REDUCED_DATA.json`)
                    .then(response => {
                        // JSON responses are automatically parsed.
                        this.interventions = response.data;
                        this.columns = Object.keys(response.data[0])
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            },
            deleteInter(index){
                var data =  this.interventions
                data.splice(index, 1)
            },
            sortByKey(key){
                if(key !== this.order.by) {
                    this.order.by = key
                }
                if(this.order.order === 'ASC') {
                    this.order.order = 'DESC'
                }else {
                    this.order.order = 'ASC'
                }
            }
        },
        mounted() {
            this.fetchData()
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
        cursor: auto;
    }

    td {
        background-color: #f9f9f9;
    }

    th, td {
        min-width: 120px;
        padding: 10px 20px;
    }

    input:checked {
        border: 1px solid blue;
    }


</style>