<template>
        <div>
            <div v-show=" ! showingUserProfile">
                <!-- Search Field card -->
                <div class="card card-default" style="border: 0;">
                    <div class="card-body">
                        <form role="form" @submit.prevent>
                            <!-- Search Field -->
                            <input type="text" id="kiosk-users-search" class="form-control"
                                   name="search"
                                   placeholder="Search By Name Or E-Mail Address..."
                                   v-model="searchForm.query"
                                   @keyup.enter="search">
                        </form>
                    </div>
                </div>

                <!-- Searching -->
                <div class="card card-default" v-if="searching">
                    <div class="card-header">Search Results</div>

                    <div class="card-body">
                        <i class="fa fa-btn fa-spinner fa-spin"></i> Searching
                    </div>
                </div>

                <!-- No Search Results -->
                <div class="card card-default" v-if=" ! searching && noSearchResults">
                    <div class="card-header">Search Results</div>

                    <div class="card-body">
                        No users matched the given criteria.
                    </div>
                </div>

                <!-- User Search Results -->
                <div class="card card-default" v-if=" ! searching && searchResults.length > 0">
                    <div class="card-header">Search Results</div>

                    <div class="table-responsive">
                        <table class="table table-valign-middle mb-0">
                            <thead>
                            <th></th>
                            <th>Name</th>
                            <th>E-Mail Address</th>
                            <th class="th-fit"></th>
                            </thead>

                            <tbody>
                            <tr v-for="searchUser in searchResults">
                                <!-- Profile Photo -->
                                <td>
                                    <img :src="searchUser.photo_url" class="spark-profile-photo" alt="User Photo" />
                                </td>

                                <!-- Name -->
                                <td>
                                    <div class="btn-table-align">
                                        {{ searchUser.name }}
                                    </div>
                                </td>

                                <!-- E-Mail Address -->
                                <td>
                                    <div class="btn-table-align">
                                        {{ searchUser.email }}
                                    </div>
                                </td>

                                <td>
                                    <!-- View User Profile -->
                                    <button class="btn btn-default" @click="showUserProfile(searchUser)">
                                        <i class="fa fa-search"></i>
                                    </button>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- User Profile Detail -->
            <div v-show="showingUserProfile">
                <profile :user="user" :plans="plans"></profile>
            </div>
        </div>
</template>

<script>
    const base = require('kiosk/users');

    import Profile from './profile'

    export default {
        name: "users",
        props: ["user"],
        components: { Profile },
        mixins: [base]
    }
</script>

<style scoped>

</style>