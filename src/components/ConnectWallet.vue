<template>

    <header class="header_con shadow-sm">
        <div class="container d-flex justify-content-between align-items-center">
            <img src="../assets/images/logo.svg" alt="" class="header_logo">
            <div class="d-flex">
                <p class="header_link">Dashboard</p>
                <p class="header_link">Team</p>
                <p class="header_link">Projects</p>
                <p class="header_link">Calender</p>
                <p class="header_link">Contact</p>
            </div>
            <div class="d-flex align-items-center">
                <div v-if="currentAccount" class="header_img position-relative">
                    <div v-on:click="account_option = !account_option">
                        <img src="../assets/images/user.png" alt="" class="me-1">
                        <span>{{ TrimAddress(currentAccount, 7) }}</span>
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                            class="bi bi-caret-down-fill" viewBox="0 0 16 16">
                            <path
                                d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z" />
                        </svg>
                    </div>
                    <div v-if="account_option" class="header_img_drop position-absolute shadow-sm">
                        <p v-on:click="DisconnectWallet()" class="header_link">Settings</p>
                    </div>
                </div>
                <button v-else v-on:click="ConnectWallet()" type="button" class="header_btn">Connect</button>
            </div>
        </div>
    </header>

    <div class="container banner_con d-flex justify-content-center">
        <div class="col-6 banner_cont position-relative shadow">
            <h1 class="banner_header">Check Balance</h1>

            <form method="post" v-on:submit.prevent="GetBalance()" class="">
                <p class="banner_error">{{ account_error }}</p>
                <input v-model="account_address" type="text" class="banner_box shadow-sm mb-4"
                    placeholder="Enter your ETH address">
                <p class="banner_result d-flex justify-content-center align-items-center">Balance: <span class="ms-2">{{
                        account_balance
                }} ETH</span></p>
                <div class="d-flex justify-content-center">
                    <button type="submit" class="banner_btn">Process</button>
                </div>
            </form>
            <div v-if="loading"
                class="banner_loading d-flex justify-content-center align-items-center position-absolute">
                <div class="spinner-border text-light" role="status">
                    <span class="visually-hidden">Loading...</span>
                </div>
            </div>
        </div>
    </div>



</template>


<script>
import { ethers } from "ethers";
const provider = window.ethereum ? new ethers.providers.Web3Provider(window.ethereum) : null;

export default {
    data() {
        return {
            currentAccount: null,
            currentContract: null,
            contractAddress: null,

            account_option: false,
            account_address: null,
            account_balance: 0,
            account_error: null,

            loading: false,
        }
    },

    methods: {
        async ConnectWallet() {

            if (!window.ethereum) {
                alert("Make sure you have metamask!");
                return;
            }
            // Connect to an account
            await provider.send("eth_requestAccounts", []).then((res) => {
                if (res.length !== 0) {
                    this.currentAccount = res[0];
                }
            }).catch((error) => {
                alert(error.message);
            });

        },

        DisconnectWallet() {

        },

        async GetBalance() {
            this.loading = true;
            this.account_error = null;
            this.account_balance = 0;
            await provider.getBalance(this.account_address).then((res) => {
                this.loading = false;
                this.account_balance = ethers.utils.formatEther(res);
            }).catch((error) => {
                this.loading = false;
                this.account_error = error.message;
            });
        },

        TrimAddress(word, to) {
            return word.slice(0, to) + '...';
        }
    },

    computed: {

    },

    mounted() {
        // this.ConnectWallet();
    }


}
</script>


<style>

</style>