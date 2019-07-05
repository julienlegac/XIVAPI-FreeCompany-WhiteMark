<template>
    <div v-if="info">
        <p v-if="info.ClassJobCategory.Name === this.jobCategory">
            <img class="stuff__info--image" v-bind:src="'https://xivapi.com' + info.Icon"> {{ level }}
        </p>

        <div v-else class="no-content"></div>
    </div>

</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';

    export default {
        name: 'ClassJob',
        data () {
            return{
                info: null,
                level: this.classJobLv,
            }
        },
        props: {
            classJobsId: Number,
            classJobLv: Number,
            jobCategory: String,
        },
        mounted () {
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/ClassJob/' + this.classJobsId + '?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;

                Array.prototype.forEach.call(document.getElementsByClassName('no-content'), function(el){
                    el.parentNode.parentNode.classList.add('d-none');
                });
            });
        },
    }
</script>