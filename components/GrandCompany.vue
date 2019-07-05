<template>
    <p v-if="info" class="grand-company">Grande compagnie : {{ info.Name_fr }}</p>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';

    export default {
        name: 'GrandCompany',
        data () {
            return{
                info: null,
            }
        },
        props: {
            gcId: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/GrandCompany/' + this.gcId + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;
            });
        },
    }

</script>