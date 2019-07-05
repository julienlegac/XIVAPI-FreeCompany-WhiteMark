<template>
    <div class="bubble-info" v-if="info">
        <div class="row">
            <div class="col-4">
                <img class="bubble-info--image" v-bind:src="'https://xivapi.com' + info.Icon">
            </div>

            <div class="col-8">
                <strong class="mb-2">{{ info.Name_fr }}</strong>
                <i class="m-1">"{{ info.Description_fr }}"</i>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';

    export default {
        name: 'MountsInfo',
        data () {
            return{
                info: null,
            }
        },
        props: {
            idMountInfo: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/mount/' + this.idMountInfo + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;
            });
        },
    }

</script>