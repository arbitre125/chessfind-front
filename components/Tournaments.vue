<template>
    <div>
        <Navbar :total="total" @newInputValue="updateInput" />
        <div class="content-container">
            <p v-if="$fetchState.pending" class="message info">
                Fetching tournaments...
            </p>
            <p v-else-if="$fetchState.error" class="message error">
                An error occurred :(
            </p>
            <p v-else-if="!filteredTournaments.length" class="message info">
                No tournaments found :(
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
            searchInput: ''
        }
    },
    computed: {
        filteredTournaments() {
            return this.tournaments.filter((t) => {
                return t.name
                    .toLowerCase()
                    .match(this.searchInput.toLowerCase())
            })
        },
        total() {
            return this.filteredTournaments.length
        }
    },
    methods: {
        updateInput(value) {
            this.searchInput = value
        }
    },
    async fetch() {
        const APItournaments = await fetch(
            'http://127.0.0.1:8000/tournaments'
        ).then((res) => res.json())
        this.total = APItournaments.total
        this.tournaments = APItournaments.tournaments
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
