<template>
    <div class="row w-100 m-auto" v-if="info">
        <div class="col-12">
          <nuxt-link to="/" rel="canonical">
            Retour
          </nuxt-link>
        </div>

        <div class="col-12 col-lg-4">
            <span v-if="info.Character.Portrait" :style="{'background-image': 'url(' + info.Character.Portrait + ')'}" class="character__portrait d-none d-lg-block"></span>
            <span v-if="info.Character.Avatar" :style="{'background-image': 'url(' + info.Character.Avatar + ')'}" class="character__portrait d-block d-lg-none"></span>
        </div>

        <div class="col-12 col-lg-8" v-if="info.Character != null">
            <b-card no-body>
                <b-tabs card>
                    <b-tab title="Information" active>
                        <b-card-text>
                            <div class="character-info__information">
                                <p v-if="info.Character.Name">{{ info.Character.Name }}</p>
                                <Title v-if="info.Character.Title" v-bind:titleId="info.Character.Title"/>
                                <p v-if="info.Character.Nameday">{{ info.Character.Nameday }}</p>
                                <Race v-if="info.Character.Race" v-bind:raceId="info.Character.Race"/>
                                <p v-if="info.Character">Lvl. {{ info.Character.ActiveClassJob.Level }}</p>
                                <ClassJob v-if="info.Character" v-bind:idClass="info.Character.ActiveClassJob.JobID"></ClassJob>
                                <GrandCompany v-if="info.Character.GrandCompany" v-bind:gcId="info.Character.GrandCompany.NameID"></GrandCompany>
                                <p v-if="info.Character.Server">Serveur : {{ info.Character.Server }}</p>
                                <p v-if="info.Character.Server">Bio : {{ info.Character.Bio }}</p>
                            </div>
                        </b-card-text>
                    </b-tab>
                    <b-tab title="Stuff">
                        <b-card-text>
                            <div class="stuff">
                                <div role="tablist">
                                    <b-card no-body class="mb-1" v-if="info.Character.GearSet.Gear">
                                        <b-card-header header-tag="header" class="p-1" role="tab">
                                            <b-button v-on:click="stuffActive = true" block href="#" v-b-toggle.accordion-1 variant="info" class="toggle-accordion">Stuff</b-button>
                                        </b-card-header>
                                        <b-collapse id="accordion-1" accordion="my-accordion" role="tabpanel" v-if="stuffActive">
                                            <b-card-body>
                                                <div>
                                                    <div class="stuff__content d-inline-block" v-for="stuff in info.Character.GearSet.Gear" :key="stuff.value">
                                                        <ItemStuff class="stuff" v-bind:idItem="stuff.ID"></ItemStuff>
                                                    </div>
                                                </div>
                                            </b-card-body>
                                        </b-collapse>
                                    </b-card>

                                    <b-card no-body class="mb-1" v-if="mirageContent != null">
                                        <b-card-header header-tag="header" class="p-1" role="tab">
                                            <b-button v-on:click="mirageActive = true" block href="#" v-b-toggle.accordion-2 variant="info" class="toggle-accordion">Mirage</b-button>
                                        </b-card-header>
                                        <b-collapse id="accordion-2" accordion="my-accordion" role="tabpanel">
                                            <b-card-body>
                                                <div>
                                                    <div class="stuff__content"   v-bind:mirageContent="stuff.Mirage" :class="{ 'd-none' : stuff.Mirage == null, 'd-inline-block' : stuff.Mirage != null}" v-for="stuff in info.Character.GearSet.Gear" :key="stuff.value">
                                                        <ItemStuff class="stuff" v-bind:idItem="stuff.Mirage"></ItemStuff>
                                                    </div>
                                                </div>
                                            </b-card-body>
                                        </b-collapse>
                                    </b-card>

                                    <b-card no-body class="mb-1" v-if="info.Character.Mounts.length > 0">
                                        <b-card-header header-tag="header" class="p-1" role="tab">
                                            <b-button v-on:click="mountsActive = true" block href="#" v-b-toggle.accordion-3 variant="info" class="toggle-accordion">Montures</b-button>
                                        </b-card-header>
                                        <b-collapse id="accordion-3" accordion="my-accordion" role="tabpanel">
                                            <b-card-body>
                                                <div v-if="mountsActive">
                                                    <div class="stuff__content d-inline-block" v-for="mount in info.Character.Mounts" :key="mount.value">
                                                        <Mounts class="stuff" v-bind:idMount="mount"></Mounts>
                                                    </div>
                                                </div>
                                            </b-card-body>
                                        </b-collapse>
                                    </b-card>

                                    <b-card no-body class="mb-1" v-if="info.Character.Minions.length > 0">
                                        <b-card-header header-tag="header" class="p-1" role="tab">
                                            <b-button v-on:click="minionsActive = true" block href="#" v-b-toggle.accordion-4 variant="info" class="toggle-accordion">Compagnons</b-button>
                                        </b-card-header>
                                        <b-collapse id="accordion-4" accordion="my-accordion" role="tabpanel">
                                            <b-card-body>
                                                <div v-if="minionsActive">
                                                    <div class="stuff__content d-inline-block" v-for="minion in info.Character.Minions" :key="minion.value">
                                                        <Companion class="stuff" v-bind:idMinion="minion"></Companion>
                                                    </div>
                                                </div>
                                            </b-card-body>
                                        </b-collapse>
                                    </b-card>
                                </div>

                            </div>
                        </b-card-text>
                    </b-tab>

                    <b-tab title="Jobs" v-if="info.Character.ClassJobs">
                        <b-card-text>
                            <div class="stuff">
                                <strong class="d-block mt-4 mb-2">Disciple de la guerre</strong>
                                <div v-for="classJob in info.Character.ClassJobs" :key="classJob.value" class="all-classJob">
                                    <ClassJobCategory v-bind:classJobsId="classJob.JobID" v-bind:classJobLv="classJob.Level" v-bind:jobCategory="'Disciple of War'"/>
                                </div>

                                <strong class="d-block mt-4 mb-2">Disciple de la magie</strong>
                                <div v-for="classJob in info.Character.ClassJobs" :key="classJob.value" class="all-classJob">
                                    <ClassJobCategory v-bind:classJobsId="classJob.JobID" v-bind:classJobLv="classJob.Level" v-bind:jobCategory="'Disciple of Magic'"/>
                                </div>

                                <strong class="d-block mt-4 mb-2">Disciple de la main</strong>
                                <div v-for="classJob in info.Character.ClassJobs" :key="classJob.value" class="all-classJob">
                                    <ClassJobCategory v-bind:classJobsId="classJob.JobID" v-bind:classJobLv="classJob.Level" v-bind:jobCategory="'Disciple of the Hand'"/>
                                </div>

                                <strong class="d-block mt-4 mb-2">Disciple de la terre</strong>
                                <div v-for="classJob in info.Character.ClassJobs" :key="classJob.value" class="all-classJob">
                                    <ClassJobCategory v-bind:classJobsId="classJob.JobID" v-bind:classJobLv="classJob.Level" v-bind:jobCategory="'Disciple of the Land'"/>
                                </div>
                            </div>
                        </b-card-text>
                    </b-tab>
                </b-tabs>
            </b-card>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import ClassJob from "~/components/ClassJob";
    import ItemStuff from "~/components/Stuff";
    import Race from "~/components/Race";
    import GrandCompany from "~/components/GrandCompany";
    import Mounts from "~/components/Mounts";
    import Title from "~/components/Title";
    import ClassJobCategory from "~/components/ClassJobCategory";
    import { setupCache } from 'axios-cache-adapter';
    import Companion from "~/components/Companion";
    import rateLimit from 'axios-rate-limit';

    export default {
        components: {Companion, ClassJobCategory, Title, Mounts, GrandCompany, ItemStuff, ClassJob, Race},
        name: 'Member',
        data () {
            return{
                info: null,
                job: null,
                active: true,
                stuffActive: false,
                mirageActive: false,
                mirageContent: true,
                mountsActive: false,
                minionsActive: false,
            }
        },
        props: {
            characterId : Number,
        },

        mounted () {
            this.$nextTick(() => {
              this.$nuxt.$loading.start()

              setTimeout(() => this.$nuxt.$loading.finish(), 500)
            });

            // Specific Member
            const cache = setupCache({
                maxAge: 15 * 60 * 1000
            });

            const api = rateLimit(axios.create({
                adapter: cache.adapter
            }), { maxRequests: 10, perMilliseconds: 1000 });

            api({
                url: 'https://xivapi.com/character/' + this.$route.params.id + '?data=AC,FR,FC,FCM,PVP&private_key=' + this.$env.API_KEY,
                method: 'get'
            }).then(async (response) =>  {
                this.info = response.data;

                document.getElementById('loading').classList.add("d-none");
            }, (error)  =>  {
                return Promise.reject(error);
            });
        },

        watch: {
            info(newInfo) {
                localStorage.info = newInfo;
            }
        }
    }

</script>
