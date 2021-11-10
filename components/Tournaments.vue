<template>
    <div>
        <Navbar
            @newInputValue="updateInput"
            @newMinDate="updateMinDate"
            @newMaxDate="updateMaxDate"
            @newRegion="updateRegion"
            @cleanFilters="cleanFilters"
        />
        <div class="content-container">
            <div v-if="$fetchState.pending">
                <TournamentSkeletonCard
                    v-for="i in skeletonsDisplay"
                    :key="i"
                />
            </div>
            <div v-else-if="$fetchState.error" class="message error">
                <img src="@/assets/icons/error.svg" class="placeholder-icon" />
                <h2>{{ $t('error.error_found') }}</h2>
            </div>
            <div v-else-if="!filteredTournaments.length" class="message info">
                <img
                    src="@/assets/icons/no_data.svg"
                    class="placeholder-icon"
                />
                <h2>{{ $t('error.no_result_found') }}</h2>
            </div>
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
        cleanFilters() {
            this.minDate = ''
            this.maxDate = ''
            this.region = ''
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
.message {
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 400px;
    border-radius: var(--border-radius);
}

.placeholder-icon {
    max-height: 200px;
    max-width: 400px;
    margin-bottom: 12px;
}
</style>
