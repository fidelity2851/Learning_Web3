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
                        <p v-on:click="DisconnectWallet()" class="header_link">Disconnect</p>
                    </div>
                </div>
                <button v-else v-on:click="ConnectWallet()" type="button" class="header_btn">Connect</button>
            </div>
        </div>
    </header>

    <button v-on:click="GetBalance()" class="header_btn">Balance</button>

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
            await provider.getBalance('0x95222290dd7278aa3ddd389cc1e1d165cc4bafe5').then((res) => {
                console.log(ethers.utils.formatEther(res))
            });
        },

        TrimAddress(word, to) {
            return word.slice(0, to) + '...';
        }
    },

    computed: {

    },

    mounted() {
        this.ConnectWallet();
    }


}
</script>


<style>
.con_btn {
    color: white;
    font-size: 15px;
    border: none;
    border-radius: 5px;
    padding: 15px 30px;
    background-color: #09a561;
}
</style>