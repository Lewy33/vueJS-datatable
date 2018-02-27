<template>
    <div class="liste">
        <button @click="deleteInter()">
            Supprimer
        </button>
        <button @click="addRow()">Ajouter une inter</button>
        <button @click="editRow()">Modifier une inter</button>
        <div id="nvlleInter" style="display: none">
            <input v-model="rows.id"  style="display: none"/>
            <input v-model="rows.first_name" placeholder="firstname"/>
            <input v-model="rows.last_name" placeholder="lastname"/>
            <input v-model="rows.titre" placeholder="titre"/>
            <textarea v-model="rows.description" placeholder="description"/>
            <input type="submit" @click="addInter()"/>
        </div>
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
            </tr>
            </thead>
            <tbody>
            <tr v-for="inter in dataByKeyAndOrder">
                <td>{{inter.id}}</td>
                <td>{{inter.first_name}}</td>
                <td>{{inter.last_name}}</td>
                <td>{{inter.titre}}</td>
                <td>{{inter.description}}</td>
                <td><input type="checkbox" @click="deleteMe(inter.id)" :checked="toDelete.indexOf(inter.id) > -1 " /></td>
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
            filterKey: String,
        },
        data() {
            return {
                interventions: [],
                columns: [],
                order: {
                    by: 'id',
                    order: 'ASC'

                },
                toDelete: [],
                rows:[],
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
                var data = this.interventions.sort(filter);
                if(this.order.order === 'ASC') {
                    return data
                }else {
                    return data.reverse()
                }
            }

        },
        methods: {
            fetchData() {
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
            deleteInter(index) {
                for (let i = 0; i < this.toDelete.length; i++) {
                    for (let j = 0; j < this.interventions.length; j++) {
                        if (this.interventions[j].id == this.toDelete[i]) {
                            this.interventions.splice(j, 1)
                        }
                    }
                }
                this.toDelete = [];
            },
            deleteMe(id) {
                if (this.toDelete.includes(id)) {
                    this.toDelete.splice(this.toDelete.indexOf(id), 1)
                } else {
                   this.toDelete.push(id)
                }
            },
            sortByKey(key) {
                if (key !== this.order.by) {
                    this.order.by = key
                }
                if (this.order.order === 'ASC') {
                    this.order.order = 'DESC'
                } else {
                    this.order.order = 'ASC'
                }
            },
            addRow() {
                var divAdd = document.getElementById('nvlleInter')
                if (divAdd.style.display == 'none') {
                    divAdd.style.display = 'block';
                } else {
                    divAdd.style.display = 'none';
                }
            },
            editRow() {

            },
            addInter() {
                var start_index = this.interventions.length;
                var number_of_elements_to_remove = 0;
                for (let i = 0; i<this.interventions.length; i++) {
                    this.rows.id = i;
                }
                var inter = Object.assign({}, this.rows);
                this.interventions.splice(start_index, number_of_elements_to_remove, inter);
            },
            editInter(){

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


</style>