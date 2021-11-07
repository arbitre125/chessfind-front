<template>
    <div>
        <Navbar @newInputValue="updateInput" />
        <div class="content-container">
            <p v-if="$fetchState.pending" class="message info">
                <TournamentSkeletonCard
                    v-for="i in skeletonsDisplay"
                    :key="i"
                />
            </p>
            <p v-else-if="$fetchState.error" class="message error">
                {{ $t('error.error-found') }}
            </p>
            <p v-else-if="!filteredTournaments.length" class="message info">
                {{ $t('error.no-result-found') }}
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
            tournaments: [],
            searchInput: '',
            skeletonsDisplay: 10
        }
    },
    async fetch() {
        const urlAPI = this.$config.apiURL
        const APItournaments = await fetch(urlAPI).then((res) => res.json())
        this.tournaments = APItournaments.tournaments
    },
    computed: {
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
        }
    }
}
</script>

<style scoped>
.content-container {
    margin-top: 40px;
}

.message {
    margin-top: 8px;
}
</style>
