<template>
    <div>
        <Navbar
            @newInputValue="updateInput"
            @newMinDate="updateMinDate"
            @newMaxDate="updateMaxDate"
            @newRegion="updateRegion"
        />
        <div class="content-container">
            <p v-if="$fetchState.pending" class="message info">
                <TournamentSkeletonCard
                    v-for="i in skeletonsDisplay"
                    :key="i"
                />
            </p>
            <p v-else-if="$fetchState.error" class="message error">
                {{ $t('error.error_found') }}
            </p>
            <p v-else-if="!filteredTournaments.length" class="message info">
                {{ $t('error.no_result_found') }}
            </p>
            <div v-else>
                <TournamentCard
                    v-for="tournament of filteredTournaments"
                    :key="tournament.link"
                    :tournament="tournament"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            skeletonsDisplay: 10,
            tournaments: [],
            searchInput: '',
            minDate: '',
            maxDate: '',
            region: ''
        }
    },
    async fetch() {
        const urlAPI = this.$config.apiURL + this.getParams
        const APItournaments = await fetch(urlAPI).then((res) => res.json())
        this.tournaments = APItournaments.tournaments
    },
    computed: {
        getParams() {
            let str = '/?'

            if (this.minDate !== '') {
                str += '&min_date=' + this.formatDate(this.minDate) + '&'
            }
            if (this.maxDate !== '') {
                str += '&max_date=' + this.formatDate(this.maxDate) + '&'
            }
            if (this.region !== '') {
                str += '&region=' + this.region + '&'
            }

            return str
        },
        filteredTournaments() {
            return this.tournaments.filter((t) => {
                return Object.keys(t).some((key) => {
                    return t[key]
                        .toLowerCase()
                        .match(this.searchInput.toLowerCase())
                })
            })
        }
    },
    methods: {
        updateInput(value) {
            this.searchInput = value
        },
        updateMinDate(value) {
            this.minDate = value
            this.$fetch()
        },
        updateMaxDate(value) {
            this.maxDate = value
            this.$fetch()
        },
        updateRegion(value) {
            this.region = value
            this.$fetch()
        },
        formatDate(date) {
            const d = new Date(date)
            let month = '' + (d.getMonth() + 1)
            let day = '' + d.getDate()
            const year = d.getFullYear()

            if (month.length < 2) month = '0' + month
            if (day.length < 2) day = '0' + day

            return [day, month, year].join('/')
        }
    }
}
</script>

<style scoped>
.content-container {
    margin-top: 40px;
}

.message {
    margin-top: 60px;
}
</style>
