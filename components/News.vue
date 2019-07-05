<template>
    <div class="row" v-if="info">
        <div class="col-12">
            <nuxt-link to="/" rel="canonical">
                Retour
            </nuxt-link>

            <div class="row">
                <div v-for="newItem in info" :key="newItem.value" class="col-12 col-md-6 col-lg-4 mb-4">
                    <div class="card card-item">
                        <img class="card-img-top" v-bind:src="newItem.Banner" alt="">
                        <div class="card-body">
                            <h5 class="card-title">{{ newItem.Title }}</h5>
                            <p class="card-text">{{ newItem.Html }}</p>
                            <a v-bind:href="'https://fr.finalfantasyxiv.com' + newItem.Url" target="_blank" class="btn btn-primary">Lire plus</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';


    export default {
        name: 'News',
        currentMember: 'News',

        data () {
            return{
                info: null
            }
        },

        mounted () {
            this.$nextTick(() => {
                this.$nuxt.$loading.start();
            });


            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/lodestone/news?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;

                setTimeout(function (){
                    var parseHTML = function(str) {
                        var tmp = document.implementation.createHTMLDocument();
                        tmp.body.innerHTML = str;
                        return tmp.body.children;
                    };

                    var elements = document.getElementsByClassName('card-text');
                    Array.prototype.forEach.call(elements, function(el){
                        el.innerHTML = parseHTML(el.textContent)[1].textContent.substring(0, 120) + '...';
                    });

                    document.getElementById('loading').classList.add("d-none");
                }, 1);

                this.$nextTick(() => {
                    setTimeout(() => this.$nuxt.$loading.finish(), 500)
                });
            });
        },
    }
</script>
