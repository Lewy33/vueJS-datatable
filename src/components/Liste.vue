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
            <button type="submit" @click="addInter()">Valider</button>
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
                <th>seléction</th>
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
                pages: 10,
            }
        },
        computed: {
            dataByKeyAndOrder(){
                let compare = function (filter) {
                    return function (a,b) { //closure
                            a = a[filter];
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

                let data = this.interventions;
                let filter = compare(this.order.by);
                data.sort(filter);
                if(this.order.order === 'ASC') {
                    return data
                }else {
                    return data.reverse()
                }
            },
            setPage(){
                let pages = 10;
                let data = this.interventions;
                console.log(data.length)
                data = data / pages;
                return pages

            },
            totalPages(){

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
                let divAdd = document.getElementById('nvlleInter')
                if (divAdd.style.display == 'none') {
                    divAdd.style.display = 'block';
                } else {
                    divAdd.style.display = 'none';
                }
            },
            editRow() {

            },
            addInter() {
                let start_index = this.interventions.length;
                let number_of_elements_to_remove = 0;
                for (let i = 0; i<this.interventions.length; i++) {
                    this.rows.id = i;
                }
                let inter = Object.assign({}, this.rows);
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
