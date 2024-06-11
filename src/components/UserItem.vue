<template>
    <div class="user-container">
        <div v-if="user.address" class="user-details">
            <h1>User Address: {{ user.address }}</h1>
            <h1>Username: {{ user.name }}</h1>
            <h1>User BIO: {{ user.bio || 'No BIO yet' }}</h1>
        </div>
        <div v-else class="user-not-found">
            <h1>User not found</h1>
        </div>
        <div v-if="user.address" class="user-image">
            <img :src="user.image" alt="User Image" />
        </div>
        <div v-if="user.address" class="user-actions">
            <button @click="getUser" class="btn">Refresh</button>
            <div v-if="isFriend">
                <button @click="sendRequest" :disabled="buttonDisabled" class="btn">{{ buttonText }}</button>
            </div>
            <div v-else>
                <h1>Already friends</h1>
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name: "user-item",
    data() {
        return {
            user: {},
            hasWatched: false,
            buttonText: "Send Friend Request",
            isFriend: false,
            buttonDisabled: false
        }
    },
    props: {
        userAddress: {
            type: String,
            require: true
        }
    },
    methods: {
        ...mapActions({
            getUserProfile: 'getUserProfile',
            connectWallet: "connectWallet",
            sendFriendRequest: "sendFriendRequest"
        }),
        async getUser() {
            console.log(this.userAddress)
            this.user = await this.getUserProfile([this.userAddress])
            if (this.user && this.user.friends) {
                this.isFriend = !this.user.friends.includes(this.$store.state.address);
            } else {
                this.user = {};
                this.isFriend = false;
            }
            console.log(this.user)
        },
        async sendRequest() {
            await this.sendFriendRequest(this.userAddress)
            this.buttonText = "Sent request"
            this.buttonDisabled = true
        }
    },
    watch: {
        '$route.params.searchAddress': 'getUser'
    },
    async mounted() {
        this.getUser()
    }
}
</script>

<style>
.user-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.user-details, .user-not-found {
    text-align: center;
    margin-bottom: 20px;
}

.user-details h1, .user-not-found h1 {
    margin: 10px 0;
}

.user-image img {
    max-width: 150px;
    border-radius: 5px;
    margin-top: 10px;
    display: block;
    margin-left: auto;
    margin-right: auto;
}

.user-actions {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.user-actions h1 {
    margin-top: 10px;
}

body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    color: #333;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.btn {
    background-color: #3498db;
    color: #fff;
    border: none;
    padding: 10px 20px;
    margin: 5px;
    cursor: pointer;
    border-radius: 5px;
}

input,
textarea {
    width: 100%;
    padding: 10px;
    margin: 5px 0;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

img {
    max-width: 150px;
    height: auto;
    border-radius: 5px;
    margin-top: 10px;
    display: block;
    margin-left: auto;
    margin-right: auto;
}

.input-field {
    position: relative;
    margin: 10px 0;
}

.input-field i {
    position: absolute;
    top: 50%;
    left: 10px;
    transform: translateY(-50%);
    color: #3498db;
}

.input-field input[type="file"] {
    padding-left: 30px;
}

.search-container {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.address-input {
    flex: 1;
    margin-right: 10px;
}
</style>