<template>
    <div class="row" v-if="info != null">
        <div class="col">
            <div class="stuff__info">
                <img class="stuff__info--image" v-bind:src="'https://xivapi.com' + info.Icon">
            </div>
        </div>

       <ItemStuffInfo v-bind:idItemInfo="info.ID"></ItemStuffInfo>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';
    import ItemStuffInfo from "./StuffInfo";

    export default {
        name: 'ItemStuff',
        components: {ItemStuffInfo},
        data () {
            return{
                info: null
            }
        },
        props: {
            idItem: Number
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            if(this.idItem != null){
                api({
                    url: 'https://xivapi.com/item/' + this.idItem + '?private_key=' + this.$env.API_KEY,
                    method: 'get'
                }).then(async (response) =>  {
                    this.info = response.data;
                });
            }
        },
    }

</script>