<template>
    <div class="row" v-if="info">
        <div v-for="server in info" :key="server.value" class="col-12 col-md-6 col-lg-4 mb-4">
            <div class="row align-item-center card-member server-status__item col-12 justify-content-center">
                <strong>
                    <span v-if="server.Status === 'Online'" class="server-status__watch online"></span>
                    <span v-else class="server-status__watch offline"></span>
                    {{ server.Title }}
                </strong>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';


    export default {
        name: 'ServerStatus',
        currentMember: 'ServerStatus',

        data () {
            return{
                info: null,
                active: false,
            }
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/lodestone/worldstatus?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;

                document.getElementById('loading-server').classList.add("d-none");
            });
        },
    }
</script>
