<template>
    <div>
        <nav class="navbar">
            <div class="search-container">
                <input v-model="searchAddress" type="text" placeholder="Search Address" name="searchAddress"
                    class="address-input" />
                <button @click="searchUser" class="btn">Search</button>
            </div>
            <div class="nav-buttons">
                <button @click="connectWallet" class="btn">Connect Metamask</button>
                <button @click="$router.push('/')" class="btn">Мой профиль</button>
                <button @click="$router.push('/requests')" class="btn">Входящие запросы</button>
            </div>
        </nav>
        <!-- Rest of your content here -->
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name: 'navbar',
    data() {
        return {
            name: "",
            bio: "",
            selectedFile: null,
            searchAddress: "",
            user: {}
        };
    },
    methods: {
        ...mapActions({
            connectWallet: "connectWallet",
            getUserProfile: "getUserProfile",
        }),
        handleFileChange(event) {
            this.selectedFile = event.target.files[0];
        },
        async getUser() {
            console.log(this.$store.state.address)
            this.user = await this.getUserProfile([this.$store.state.address])
        },
        async searchUser() {
            this.$router.push(`/user/${this.searchAddress}`)
            this.searchAddress = ""
        }
    },
};
</script>

<style scoped>
/* Add some basic styling to the page */
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

/* Style for buttons */
.btn {
    background-color: #3498db;
    color: #fff;
    border: none;
    padding: 10px 20px;
    margin: 5px;
    cursor: pointer;
    border-radius: 5px;
}

/* Style for input fields */
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

/* Style for the navbar */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    background-color: #ff5555;
    /* Reddish color */
}

/* Style for the address input in the navbar */
.address-input {
    flex: 1;
    margin-right: 10px;
}

/* Style for the navigation buttons in the navbar */
.nav-buttons {
    display: flex;
    gap: 10px;
}
</style>