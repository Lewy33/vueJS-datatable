<template>
    <div class="liste">
        <ul>
            <li v-for="lists in list">{{post}}</li>
        </ul>
    </div>
</template>

<script>
    import * as axios from "axios";

    export default {
        name: 'Liste',
        props: {
            msg: String
        },
        data() {
            return {
                list: [],
                errors: []
            }
        },
        mounted() {
            axios.get(`https://raw.githubusercontent.com/mdubourg001/datatable_vuejs/master/src/assets/MOCK_DATA.json`)
                .then(response => {
                    // JSON responses are automatically parsed.
                    this.list = response.data
                })
                .catch(e => {
                    this.errors.push(e)
                })
        }
    }
</script>