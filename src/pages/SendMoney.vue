<template>
    <div class="container">
        <div class="main-body">
            <div class="card">
                <div class="card-body">
                    <div class="d-flex flex-column align-items-center text-center">
                        <div id="sendmoney">
                            <label for="receiverAddress">Receiver Address:</label>
                            <input type="text" id="receiverAddress" v-model="receiverAddress">

                            <label for="amount">Amount (ETH):</label>
                            <input type="text" id="amount" v-model="amount">

                            <button @click="send">Send ETH</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { mapActions } from 'vuex'
export default {
    name: 'MainView',
    data() {
        return {
            receiverAddress: '',
            amount: '0',
        };
    },
    methods: {
        ...mapActions({
            convertToWei: "convertToWei"
        }),
        async send() {
            console.log("wtf")
            console.log(this.receiverAddress + " " + this.amount)
            let amountInWei = await this.convertToWei(this.amount)
            console.log(amountInWei)
            try {
                await ethereum.request({
                    method: "eth_sendTransaction",
                    params: [
                        {
                        from: this.$store.state.address,
                        to: this.receiverAddress,
                        value: amountInWei,
                        gasLimit: '0x5028',
                        maxPriorityFeePerGas: '0x3b9aca00',
                        maxFeePerGas: '0x2540be400',
                        },
                    ],
                })
                document.getElementById("sendmoney").innerHTML = "<h3>Successfully sent!</h3>";
            }
            catch (err) {
                document.getElementById("sendmoney").innerHTML = "<h3>Transaction failed</h3>";
            }
        }
    },
};
</script>
<style scoped>
body {
    font-family: 'Arial', sans-serif;
    background-color: #4caf50;
    /* Green color */
    color: #fff;
    /* White text color for better contrast */
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.main-body {
    padding: 15px;
}

.card {
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, .1), 0 1px 2px 0 rgba(0, 0, 0, .06);
    position: relative;
    display: flex;
    flex-direction: column;
    min-width: 0;
    word-wrap: break-word;
    background-color: #fff;
    background-clip: border-box;
    border: 0 solid rgba(0, 0, 0, .125);
    border-radius: .25rem;
}

.card-body {
    flex: 1 1 auto;
    min-height: 1px;
    padding: 1rem;
}

.gutters-sm {
    margin-right: -8px;
    margin-left: -8px;
}

.gutters-sm>.col,
.gutters-sm>[class*="col-"] {
    padding-right: 8px;
    padding-left: 8px;
}

.mb-3,
.my-3 {
    margin-bottom: 1rem !important;
}

.bg-gray-300 {
    background-color: #e2e8f0;
}

.h-100 {
    height: 100% !important;
}

.shadow-none {
    box-shadow: none !important;
}

/* Add some basic styling to the page */
.container {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    color: #333;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    width: 100%;
}

/* Style for buttons */
.btn {
    background-color: #ff5555;
    /* Reddish color */
    color: #fff;
    border: none;
    padding: 15px 30px;
    /* Larger padding */
    margin: 10px;
    /* Larger margin */
    cursor: pointer;
    border-radius: 8px;
    /* Rounded corners */
}

/* Style for input fields */
input,
textarea {
    width: 100%;
    padding: 15px;
    /* Larger padding */
    margin: 10px 0;
    /* Larger margin */
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 8px;
    /* Rounded corners */
    font-size: 18px;
    /* Larger font size */
}

/* Style for the image */
img {
    max-width: 150px;
    height: auto;
    border-radius: 8px;
    /* Rounded corners */
    margin-top: 15px;
    /* Larger margin */
    display: block;
    margin-left: auto;
    margin-right: auto;
}

/* Style for the input field with the file icon */
.input-field {
    position: relative;
    margin: 15px 0;
    /* Larger margin */
}

.input-field i {
    position: absolute;
    top: 50%;
    left: 15px;
    /* Larger left position */
    transform: translateY(-50%);
    color: #ff5555;
    /* Reddish color */
}

/* Style for the file input */
.input-field input[type="file"] {
    padding-left: 45px;
    /* Larger padding */
}

/* Style for the container of the search input and button */
.search-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
    /* Larger margin */
}

/* Adjust the styles for the address input to fit in the same line */
.address-input {
    flex: 1;
    margin-right: 15px;
    /* Larger margin */
}
</style>