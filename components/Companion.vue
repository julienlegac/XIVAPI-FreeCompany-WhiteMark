<template>
    <div class="row" v-if="info">
        <div class="col-12">
            <div class="stuff__info" v-if="info">
                <img class="stuff__info--image" v-bind:src="'https://xivapi.com' + info.IconSmall">
            </div>

            <CompanionInfo v-bind:idMinionInfo="info.ID"></CompanionInfo>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';
    import CompanionInfo from "./CompanionInfo";

    export default {
        name: 'Companion',
        components: {CompanionInfo},
        data () {
            return{
                info: null,
            }
        },
        props: {
            idMinion: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/companion/' + this.idMinion + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;
            });
        },
    }

</script>