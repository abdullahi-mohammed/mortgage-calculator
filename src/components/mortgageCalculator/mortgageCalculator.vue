<style>
@import url('/src/assets/main.css');
@import url('./style.module.css');
</style>

<template>
    <main>
        <section class="calculator text-[#4e6e7e]">
            <div class="header">
                <h1 class="font-bold text-[#122f3f]">Mortgage Calculator</h1>
                <button class="underline underline-offset-2" @click="clearAllHandler" type="button">Clear All</button>
            </div>
            <div>
                <form @submit.prevent="calculateRepaymentHandler" action="">
                    <div>
                        <label for="amount" class="text-[12px] font-medium">Mortgage Amount</label>
                        <div class="inputWrapper">
                            <span class="inputUnit font-bold text-[14px]">$</span>
                            <input class="font-bold text-[#122f3f]" @change="updateState($event, amount)"
                                :value="amount" type="number" id="amount">
                        </div>
                    </div>
                    <div class="factors mt-3">
                        <div>
                            <label for="term" class="text-[12px] font-medium">Mortgage Term</label>
                            <div class="inputWrapper flex-row-reverse">
                                <span class="inputUnit font-bold text-[14px]">years</span>
                                <input class="font-bold text-[#122f3f]" v-on:change="updateState($event, term)"
                                    :value="term" type="number" id="term">
                            </div>
                        </div>
                        <div>
                            <label for="rate" class="text-[12px] font-medium">Interest Rate</label>
                            <div class="inputWrapper flex-row-reverse">
                                <span class="inputUnit font-bold text-[14px]">%</span>
                                <input class="font-bold text-[#122f3f]" @change="updateState($event, rate)"
                                    :value="rate" type="number" id="rate">
                            </div>
                        </div>
                    </div>

                    <div class="type mt-3">
                        <h4 class="text-[12px] font-medium">Mortgage Type</h4>
                        <div class="inputWrapper">
                            <input @change="updateType($event, repayments)" type="radio" name="type" id="repayment">
                            <label for="repayment" class="font-bold text-[#122f3f]">Repayments</label>
                        </div>
                        <div class="inputWrapper">
                            <input class="accent-[#d7da2f]" @change="updateType($event, interest)" type="radio"
                                name="type" id="interest">
                            <label for="interest" class="font-bold text-[#122f3f]">Interest Only</label>
                        </div>
                    </div>
                    <button class="py-3 px-5 bg-[#d7da2f] rounded-full mt-3 flex gap-2 items-center" type="submit">
                        <img width="20" src="../../assets/icon-calculator.svg" alt=""><span
                            class="font-bold text-[#122f3f] text-[14px]">Calculate Repayments</span>
                    </button>
                </form>
            </div>
        </section>
        <section class="results">
            <div v-if="monthlyResult == '' || timelyResult == ''"
                class="flex items-center justify-center gap-2 flex-col h-full">
                <img class="object-cover" width="150" src="../../assets/illustration-empty.svg" alt="">
                <h4 class="font-medium">Results shown here</h4>
                <p class="text-balance text-center text-[12px] text-[#9abed5]">Complete the form and
                    "calculate repayments" to see what
                    your
                    monthly repayments
                    would be</p>
            </div>
            <div v-else>
                <h1 class="font-bold">Your results</h1>
                <p class="text-[12px] text-[#9abed5] mt-2">Your results are shown below based on the information you
                    provided. To
                    adjust the results, edit the form
                    and click "calculate repayments" again.</p>

                <div class="w-full p-4 mt-4 bg-[#0c202b] rounded-lg overflow-hidden relative">
                    <div class="absolute top-0 left-0 right-0 bg-[#d7da2f] h-[8px]"></div>
                    <h4 class="mt-2 text-[12px] font-medium text-[#9abed5]">Your monthly repayments</h4>
                    <p class="text-[32px] font-bold text-[#d7da2f]">{{ monthlyResult }}</p>
                    <hr class="border-[#9abed5] my-4">
                    <p class="mt-2 text-[12px] font-medium text-[#9abed5]">Total you will repay over time</p>
                    <p class="text-[18px] font-bold">{{ timelyResult }}</p>
                </div>
            </div>


        </section>
    </main>
</template>

<script>
import { reactive, toRefs } from 'vue'
export default {
    setup() {

        const initialData = {
            amount: '',
            term: '',
            rate: '',
            monthlyResult: '',
            timelyResult: '',
            type: ''
        }

        const state = reactive(initialData)

        const clearAllHandler = () => {
            state.amount = ''
            state.term = ''
            state.rate = ''
            state.monthlyResult = ''
            state.timelyResult = ''
            state.type = ''
            console.log('clear handler clicked');

        }

        // var M; //monthly mortgage payment
        // var P = 400000; //principle / initial amount borrowed
        // var I = 3.5 / 100 / 12; //monthly interest rate
        // var N = 30 * 12; //number of payments months

        //monthly mortgage payment
        const M = monthlyPayment(state.amount, state.term * 12, state.rate / 100 / 12);

        console.log(M);

        function monthlyPayment(p, n, i) {
            return p * i * (Math.pow(1 + i, n)) / (Math.pow(1 + i, n) - 1);
        }

        const calculateRepaymentHandler = () => {
            if (state.type == 'repayment') {
                state.monthlyResult = M
                state.timelyResult = 7845
            } else if (state.type == 'interest') {
                state.monthlyResult = M
                state.timelyResult = 75
            } else {
                console.log('error calculating repayment');
                state.monthlyResult = ''
                state.timelyResult = ''
            }

        }


        const updateState = (e, type) => {
            console.log(e.target.id, state[e.target.id]);

            state[e.target.id] = e.target.value
        }
        const updateType = (e, type) => {
            console.log(e.target.name, state[e.target.name]);

            state[e.target.name] = e.target.id
        }
        console.log(state);

        return {
            ...toRefs(state),
            clearAllHandler,
            calculateRepaymentHandler,
            updateState,
            updateType
        }
    }
}
</script>