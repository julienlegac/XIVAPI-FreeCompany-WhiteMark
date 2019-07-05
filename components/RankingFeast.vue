<template>
    <div class="row" v-if="info">
        <div v-bind:id="rank.Character.ID" v-on:click="memberID = rank.Character.ID, active = true" v-for=" rank in info.Results " :key="rank.value" class="list-members__item col-12 col-sm-6 col-lg-4">
            <div class="row align-item-center card-member col-12">
                <div class="col-12 col-lg-4">
                    <span class="list-members__item--photo" v-bind:style="{ backgroundImage: 'url(' + rank.Character.Avatar + ')' }"></span>
                </div>

                <div class="col-12 col-lg-8">
                    <p><strong>Name :</strong> {{ rank.Character.Name }}</p>
                    <p><strong>Rank :</strong> #{{ rank.Leaderboard.Rank }} - <img v-bind:src="rank.Leaderboard.RankImage" alt=""></p>
                    <p><strong>Server :</strong> {{ rank.Character.Server }}</p>
                </div>
            </div>
        </div>

        <div v-if="active" class="character">
            <div class="character-info">
                <div class="character-info__container">
                    <a href="#" class="close-character" v-on:click="active = false">Back</a>
                    <Member v-if="active" v-bind:characterId="memberID"/>
                </div>
            </div>

            <div class="loading" id="loading">
                <div class="loading__content">
                    <img src="~/assets/loader.gif"/>
                    Please wait Kuppo...
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import { setupCache } from 'axios-cache-adapter';
    import rateLimit from 'axios-rate-limit';
    import Member from "./Member";


    export default {
        name: 'RankingFeast',
        components: {Member},
        currentMember: 'RankingFeast',

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
                url: 'https://xivapi.com/lodestone/feasts?private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;

                document.getElementById('loading-ranking').classList.add("d-none");
            });
        },
    }
</script>
