<template>
    <div>
        Cat stuff
         <b-table striped show-empty :items="filtered" :fields="fields" @row-clicked="showDetails">
            <template slot="top-row" slot-scope="{ fields }">
                <td v-for="field in fields" :key="field.key">
                    <input v-model="filters[field.key]" :placeholder="field.label">
                </td>
            </template>
        </b-table>
        <b-modal ref="my-modal" hide-footer title="Using Component Methods">
            <div class="d-block text-center">
                <h3>{{catfact.text}}</h3>
                {{catfact.user}}
                {{catfact.upvotes}}
            </div>
        </b-modal>
    </div>
</template>

<script>
export default {
    methods: {
        showDetails(record, index){
            this.catfact = this.catfacts.all[index];
            this.$refs['my-modal'].show();
        }
    },
    computed: {
        filtered () {
        const filtered = this.catfacts.all.filter(item => {
            return Object.keys(this.filters).every(key =>
                String(item[key]).includes(this.filters[key]))
      })
      return filtered.length > 0 ? filtered : [{
        text: '',
        user: '',
        upvotes: ''
      }]
     }
    },
    data() {
        return {
            fields: [
                {
                    key: 'text',
                    sortable: true
                },
                {
                    key: 'user',
                    sortable: false
                },
                {
                    key: 'upvotes',
                    sortable: true
                }
            ], 
            catfacts: [],
            catfact: {},
            filters: {
                text: '',
                user: '',
                upvotes: ''
            }
        }
    },
    async fetch() {
        try {
            this.catfacts = await fetch('https://cat-fact.herokuapp.com/facts').then(res => res.json());
        }catch(e){
            console.log(e);
        }
    }
}
</script>