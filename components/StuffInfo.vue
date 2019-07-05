<template>
    <div class="bubble-info" v-if="info">
        <div class="row">
            <div class="col-4">
                <img class="bubble-info--image" v-bind:src="'https://xivapi.com' + info.Icon">
            </div>

            <div class="col-8">
                <strong class="mb-2 mt-1" v-if="info.Name">{{ info.Name_fr }}</strong>
                <p class="m-1" v-if="info.ClassJobCategory.Name">{{ info.ClassJobCategory.Name_fr }}</p>
                <p class="m-1" v-if="info.LevelEquip">Lv : {{ info.LevelEquip }}</p>
                <p class="m-1" v-if="info.LevelItem">iLv : {{ info.LevelItem }}</p>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';

    export default {
        name: 'ItemStuffInfo',
        data () {
            return{
                info: null,
            }
        },
        props: {
            idItemInfo: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            if(this.idItemInfo != null){
                api({
                    url: 'https://xivapi.com/item/' + this.idItemInfo + '?private_key=' + this.$env.API_KEY,
                    method: 'get'
                }).then(async (response) =>  {
                    this.info = response.data;
                });
            }
        },
    }

</script>