<template>
        <div>
            <!-- Loading Indicator -->
            <div class="row" v-if="loading">
                <div class="col-md-12">
                    <div class="card card-default">
                        <div class="card-body">
                            <i class="fa fa-btn fa-spinner fa-spin"></i> Loading
                        </div>
                    </div>
                </div>
            </div>

            <!-- User Profile -->
            <div v-if=" ! loading && profile">
                <div class="row">
                    <div class="col-md-12">
                        <div class="card card-default">
                            <div class="card-header">
                                <div class="btn-table-align">
                                    <i class="fa fa-btn fa-times" style="cursor: pointer;" @click="showSearch"></i>
                                    {{ profile.name }}
                                </div>
                            </div>

                            <div class="card-body">
                                <div class="row">
                                    <!-- Profile Photo -->
                                    <div class="col-md-3 text-center">
                                        <img :src="profile.photo_url" class="spark-profile-photo-xl" alt="Profile Photo" />
                                    </div>

                                    <div class="col-md-9">
                                        <!-- Email Address -->
                                        <p>
                                            <strong>Email Address:</strong> <a :href="'mailto:'+profile.email"> {{ profile.email }}</a>
                                        </p>

                                        <!-- Joined Date -->
                                        <p>
                                            <strong>Joined:</strong> {{ profile.created_at | datetime }}
                                        </p>

                                        <!-- Subscription -->
                                        <p>
                                            <strong>Subscription:</strong>

                                            <span v-if="activePlan(profile)">
                                            <a :href="customerUrlOnBillingProvider(profile)" target="_blank">
                                                {{ activePlan(profile).name }} ({{ activePlan(profile).interval | capitalize }})
                                            </a>
                                        </span>

                                            <span v-else>
                                            None
                                        </span>
                                        </p>

                                        <!-- Total Revenue -->
                                        <p>
                                            <strong>Total Revenue:</strong> {{ revenue | currency(spark.currencySymbol) }}
                                        </p>
                                    </div>
                                </div>
                            </div>

                            <div class="card-footer card-flush text-right">
                                <button class="btn btn-primary btn-sm" v-if="spark.usesStripe && profile.stripe_id" @click="addDiscount(profile)">
                                    Apply Discount
                                </button>

                                <button class="btn btn-default btn-sm" @click="impersonate(profile)" :disabled="user.id === profile.id">
                                    Impersonate
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Teams -->
                <div class="row" v-if="spark.usesTeams && profile.owned_teams.length > 0">
                    <div class="col-md-12">
                        <div class="card card-default">
                            <div class="card-header">
                                Teams
                            </div>

                            <div class="card-body">
                                <table class="table table-borderless m-b-none">
                                    <thead>
                                    <th></th>
                                    <th>Name</th>
                                    <th>Subscription</th>
                                    </thead>

                                    <tbody>
                                    <tr v-for="team in profile.owned_teams">
                                        <!-- Photo -->
                                        <td>
                                            <img :src="team.photo_url" class="spark-profile-photo" alt="Team Photo" />
                                        </td>

                                        <!-- Team Name -->
                                        <td>
                                            <div class="btn-table-align">
                                                {{ team.name }}
                                            </div>
                                        </td>

                                        <!-- Subscription -->
                                        <td>
                                            <div class="btn-table-align">
                                                <span v-if="activePlan(team)">
                                                    <a :href="customerUrlOnBillingProvider(team)" target="_blank">
                                                        {{ activePlan(team).name }} ({{ activePlan(team).interval | capitalize }})
                                                    </a>
                                                </span>

                                                <span v-else>
                                                    None
                                                </span>
                                            </div>
                                        </td>
                                    </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Apply Discount Modal -->
            <div>
                <modal></modal>
            </div>
        </div>
</template>

<script>
    const base = require('kiosk/profile');

    import Modal from './modals/add-discount'

    export default {
        name: "profile",
        props: ["user", "plans"],
        components: { Modal },
        mixins: [base]
    }
</script>

<style scoped>

</style>