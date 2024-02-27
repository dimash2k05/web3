<template>
        <div>
        <img v-if="this.user.image" :src="this.user.image" alt="Pinata Image" />
        <div v-else>
            <p>No profile picture yet</p>
        </div>
    </div>
    <div>
        <h1>Address: {{ this.userAddress }}</h1>
        <h1>Username: {{ this.user.name }}</h1>
        <button @click="accept">Accept Request</button>
        <button @click="decline">Decline Request</button>
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name: 'request-item',
    data() {
        return {
            user: {}
        };
    },
    props: {
        userAddress: {
            type: String,
            required: true,
        },
    },
    methods: {
        ...mapActions({
            getUserProfile: "getUserProfile",
            acceptFriendRequest: "acceptFriendRequest",
            declineFriendRequest: "declineFriendRequest"
        }),
        async getUser() {
            console.log(this.userAddress)
            this.user = await this.getUserProfile([this.userAddress])
            console.log(this.user)
        },
        async accept(){
            await this.acceptFriendRequest([this.userAddress])
        },
        async decline(){
            await this.declineFriendRequest([this.userAddress])
        }
    },
    async mounted() {
        this.getUser()
    },
    watch: {
        'this.userAddress': 'getUser'
    }
}
</script>

<style></style>