<template>
    <div>
        <div v-show="!started">
            <div class="form-group">
                <label for="max_tickets_amount">Всего билетов</label>
                <input v-model="maxTickets" class="form-control" id="max_tickets_amount"
                       placeholder="Enter max tickets to generate" type="number"/>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" v-on:click="startTelling">Начать</button>
            </div>
        </div>
        <div v-show="started" class="mb-3">
            <h1 class="current-ticket" v-bind:class="{ 'current-ticket-confirmed': currentTicketConfirmed }">{{ currentTicket }}</h1>
            <h1 v-show="currentTicket === undefined"
                class="current-ticket">Н/Д</h1>
            <button v-on:click="tellTicket" :disabled="ticketJar.length < 1" class="btn btn-primary btn-lg btn-block">Тянуть</button>
            <hr/>
            <button v-on:click="confirmTicket" :disabled="currentTicketConfirmed || currentTicket === undefined" class="btn btn-success btn-lg">
                Подтвердить
            </button>
        </div>
        <div v-show="ticketJar.length <= 0 && started" class="mb-3">
            <p>
                Не осталось билетов. Начните сначала.
            </p>
            <button class="btn btn-danger" v-on:click="reset">Сброс</button>
        </div>
        <nav class="navbar fixed-bottom navbar-light bg-light">
            <a class="btn btn-outline-danger" href="#" v-on:click="reset">Сброс</a>
            <div class="float-right">
                {{ticketJar}}->{{ voidedTickets }}
            </div>
        </nav>
    </div>
</template>

<script>
    const getRandomInt = (max) => {
        return Math.floor(Math.random() * Math.floor(max));
    }

    export default {
        name: "TicketTeller",
        props: {
            maxTickets: Number,
            voidedTickets: {
                type: Array,
                default: () => []
            },
            ticketJar: {
                type: Array,
                default: () => []
            },
            started: {
                type: Boolean,
                default: () => false,
            },
            currentTicket: Number,
            currentTicketConfirmed: Boolean,
        },
        mounted() {
            if(localStorage.getItem('ticket-teller')) {

                const {
                    maxTickets,
                    voidedTickets,
                    ticketJar,
                    started,
                    currentTicket,
                    currentTicketConfirmed
                } = JSON.parse(localStorage.getItem('ticket-teller'))

                this.maxTickets = maxTickets;
                this.voidedTickets = voidedTickets;
                this.ticketJar = ticketJar;
                this.started = started;
                this.currentTicket = currentTicket;
                this.currentTicketConfirmed = currentTicketConfirmed;
            }
        },
        methods: {
            save() {
                localStorage.setItem('ticket-teller', JSON.stringify({
                    maxTickets: this.maxTickets,
                    voidedTickets: this.voidedTickets,
                    ticketJar: this.ticketJar,
                    started: this.started,
                    currentTicket: this.currentTicket,
                    currentTicketConfirmed: this.currentTicketConfirmed,
                }))
            },
            startTelling() {
                this.started = true;

                const range = [];

                for (let i = 1; i <= this.maxTickets; i++) {
                    range.push(i);
                }

                this.currentTicketConfirmed = false;
                this.ticketJar = range;

                this.save();
            },
            tellTicket() {

                let current = null;

                do {

                    current = this.ticketJar[getRandomInt(this.ticketJar.length)];

                } while (this.ticketJar.length > 1 && current === this.currentTicket);

                this.currentTicket = current;
                this.currentTicketConfirmed = false;

                this.save();
            },
            confirmTicket() {
                const index = this.ticketJar.indexOf(this.currentTicket);
                if (index > -1) {
                    this.ticketJar.splice(index, 1);
                }

                console.log(this.currentTicket);

                if(this.voidedTickets.indexOf(this.currentTicket) === -1 && this.currentTicket !== undefined) {
                    this.voidedTickets.push(this.currentTicket);
                }

                this.currentTicketConfirmed = true;

                this.save();
            },
            reset() {
                this.currentTicket = undefined;
                this.voidedTickets = [];
                this.ticketJar = [];
                this.started = false;

                this.save();
            }
        }
    }
</script>

<style scoped>
    .btn-primary {
        background-color: #014ecb;
    }

    .btn-danger {
        background-color: #dd1c1a;
    }

    .btn-success {
        background-color: #248232;
    }

    .btn-outline-danger:hover {
        background-color: #dd1c1a;
    }

    .current-ticket {
        color: #040f0f;
        font-size: 21rem;
        text-align: center;
    }

    .current-ticket-confirmed {
        color: #e62325;
    }

</style>