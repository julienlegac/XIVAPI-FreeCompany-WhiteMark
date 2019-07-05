<template>
    <div class="row" v-if="info">
        <div class="col-12">
            <div class="stuff__info" v-if="info">
                <img class="stuff__info--image" v-bind:src="'https://xivapi.com' + info.IconSmall">
            </div>

            <MountsInfo v-bind:idMountInfo="info.ID"></MountsInfo>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';
    import MountsInfo from "./MountsInfo";

    export default {
        name: 'Mounts',
        components: {MountsInfo},
        data () {
            return{
                info: null,
            }
        },
        props: {
            idMount: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/mount/' + this.idMount + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;
            });
        },
    }

</script>