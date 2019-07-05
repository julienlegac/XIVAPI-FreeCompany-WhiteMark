<template>
    <div class="col-12 list-members center">
        <div class="row">
            <div class="col-12 fc-info">
                <div class="container">
                    <div class="row align-item-center">
                        <div class="col-lg-1 col-0"></div>
                        <div class="col-lg-4 col-12">
                            <div class="logo-container">
                                <div v-for="logo in fcCrest" :key="logo.value" class="clearfix">
                                    <img v-bind:src="logo" class="logo" alt="Logo">
                                </div>
                            </div>
                        </div>

                        <div class="col-lg-7 col-12">
                            <h1 class="title">{{ fcName }}</h1>
                            <p>Le recrutement est <span class="lowercase" v-if="fcRecruitment === 'Open'">ouvert</span><span class="lowercase" v-if="fcRecruitment === 'Closed' || fcRecruitment === 'Close'">fermé</span> !</p>
                            <p>Création : <span id="formed"> {{ moment.unix(fcFormed).locale('fr').format("Do MMMM YYYY")}} </span></p>
                            <p>Serveur: {{ fcServer }}</p>
                            <address>{{ fcEstatePlot }}</address>
                        </div>
                    </div>
                </div>
            </div>

            <nav class="nav-menu mb-4">
                <div class="container">
                    <div class="row">
                        <div class="col nav-menu--item">
                          <nuxt-link to="/news" rel="canonical">
                            Actualités FFXIV
                          </nuxt-link>
                        </div>
                    </div>
                </div>
            </nav>

            <div class="list-members">
                <div class="container">
                    <div class="row">
                        <div v-for="members in fcMember" :key="members.value" class="list-members__item col-12 col-sm-6 col-lg-4" v-bind:id="members.ID">
                            <nuxt-link :to="{path: '/character/' + members.ID }" rel="canonical">
                                <div class="row align-item-center card-member col-12">
                                    <div class="col-12 col-lg-4">
                                        <span class="list-members__item--photo" v-bind:style="{ backgroundImage: 'url(' + members.Avatar + ')' }"></span>
                                    </div>

                                    <div class="col-12 col-lg-8">
                                        <h2><strong>Nom :</strong> {{ members.Name }}</h2>
                                        <p><strong>Rang :</strong> <img v-bind:src="members.RankIcon" alt="Rank Icon"> {{ members.Rank }}</p>
                                    </div>
                                </div>
                            </nuxt-link>
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
    import moment from 'moment';

    export default {
        name: 'ListMembers',
        components: {},
        currentMember: 'Member',

        data () {
            return{
                moment: moment,
                fcName: null,
                fcCrest: null,
                fcRecruitment: null,
                fcFormed: null,
                fcServer: null,
                fcEstatePlot: null,
                fcMember: null,
            }
        },


        mounted () {
            this.$nextTick(() => {
                this.$nuxt.$loading.start();
            });


            // API FC Members
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 30, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/FreeCompany/' + this.$env.FREECOMPANY_ID + '?data=FCM&private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.fcName = response.data.FreeCompany.Name;
                this.fcCrest = response.data.FreeCompany.Crest;
                this.fcRecruitment = response.data.FreeCompany.Recruitment;
                this.fcFormed = response.data.FreeCompany.Formed;
                this.fcServer = response.data.FreeCompany.Server;
                this.fcEstatePlot = response.data.FreeCompany.Estate.Plot;
                this.fcMember = response.data.FreeCompanyMembers;

                this.$nextTick(() => {
                    setTimeout(() => this.$nuxt.$loading.finish(), 500)
                });
            });
        },
    }
</script>
