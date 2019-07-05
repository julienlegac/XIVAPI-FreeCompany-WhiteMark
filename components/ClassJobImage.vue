<template>
    <img v-if="info" class="picto-classJob" v-bind:src="'https://xivapi.com' + info.Icon">
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';

    export default {
        name: 'ClassJobImage',
        data () {
            return{
                info: null,
            }
        },
        props: {
            idClass: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/ClassJob/' + this.idClass + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;
            });
        },
    }

</script>