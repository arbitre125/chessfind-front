<template>
    <div>
        <Navbar
            :loading="loading"
            @newInputValue="updateInput"
            @newMinDate="updateMinDate"
            @newMaxDate="updateMaxDate"
            @newMinDays="updateMinDays"
            @newMaxDays="updateMaxDays"
            @newRegions="updateRegions"
            @newValidFIDE="updateValidFIDE"
            @cleanFilters="cleanFilters"
        />
        <div class="content-container">
            <div v-if="loading">
                <TournamentSkeletonCard class="header-info" />
                <TournamentSkeletonCard v-for="i in displayPerPage" :key="i" />
            </div>
            <div v-else-if="error" class="message error">
                <img src="@/assets/icons/error.svg" class="placeholder-icon" />
                <h2>{{ $t('error.error_found') }}</h2>
            </div>
            <div v-else-if="!tournaments.length" class="message info">
                <img
                    src="@/assets/icons/no_data.svg"
                    class="placeholder-icon"
                />
                <h2>{{ $t('error.no_result_found') }}</h2>
            </div>
            <div v-else>
                <div class="header-info">
                    <div class="header-results">
                        <b>{{ totalTournaments }}</b>
                        {{ $t('tournaments').toLowerCase() }}
                    </div>
                    <div class="header-sorting">
                        <div class="sorting">
                            {{ $t('page') }}
                            <select v-model="displayPerPage">
                                <option v-for="page in pageOptions" :key="page">
                                    {{ page }}
                                </option>
                            </select>
                        </div>

                        <div class="sorting">
                            {{ $t('sort_by') }}
                            <select v-model="sorting">
                                <option
                                    v-for="item in sortOptions"
                                    :key="item.display"
                                    :value="{
                                        value: item.value,
                                        dir_desc: item.desc
                                    }"
                                >
                                    {{ item.display }}
                                </option>
                            </select>
                        </div>
                    </div>
                </div>
                <TournamentCard
                    v-for="tournament of tournaments"
                    :key="tournament.link"
                    :tournament="tournament"
                />
                <div class="pagination">
                    <button
                        class="pagination-btn"
                        :disabled="currentPage == 1"
                        @click="prevPage"
                    >
                        <img src="@/assets/icons/arrow_left.svg" />
                    </button>
                    <input
                        v-model="currentPage"
                        type="number"
                        :min="1"
                        :max="maxPages"
                        @change="updatePage"
                    />
                    / {{ maxPages }}
                    <button
                        class="pagination-btn"
                        :disabled="currentPage == maxPages"
                        @click="nextPage()"
                    >
                        <img src="@/assets/icons/arrow_right.svg" />
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            loading: false,
            error: false,
            tournaments: [],
            totalTournaments: 0,
            displayPerPage: 5,
            currentPage: 1,
            sorting: {
                value: 'start',
                dir_desc: false
            },
            searchInput: '',
            minDate: '',
            maxDate: '',
            minDays: '',
            maxDays: '',
            regions: [],
            onlyValidByFIDEelo: '',
            awaitingInput: false
        }
    },
    async fetch() {
        this.loading = true

        await this.$axios
            .post(this.$config.apiURL, this.getParams)
            .then((response) => {
                this.tournaments = response.data.tournaments
                this.totalTournaments = response.data.total
                this.loading = false
            })
            .catch(() => {
                this.loading = false
                this.error = true
            })
    },
    computed: {
        maxPages() {
            return Math.ceil(this.totalTournaments / this.displayPerPage)
        },
        pageOptions() {
            return [5, 10, 25, 50, 100]
        },
        sortOptions() {
            return [
                { value: 'start', desc: false, display: 'Soonest start day' },
                { value: 'start', desc: true, display: 'Latest start day' },
                { value: 'end', desc: false, display: 'Soonest end day' },
                { value: 'end', desc: true, display: 'Latest end day' },
                { value: 'rounds', desc: true, display: 'More rounds' },
                { value: 'rounds', desc: false, display: 'Less rounds' },
                {
                    value: 'days_duration',
                    desc: true,
                    display: 'More duration'
                },
                {
                    value: 'days_duration',
                    desc: false,
                    display: 'Less duration'
                },
                {
                    value: 'average_elo',
                    desc: true,
                    display: 'More average ELO'
                },
                {
                    value: 'average_elo',
                    desc: false,
                    display: 'Less average ELO'
                }
            ]
        },
        getParams() {
            const params = {}
            params.mode = 'cors'
            params.headers = {
                'Access-Control-Allow-Origin': '*'
            }
            params.display_per_page = this.displayPerPage
            params.current_page = this.currentPage
            params.sorting_value = this.sorting.value
            params.sorting_dir_desc = this.sorting.dir_desc
            params.search_text = this.searchInput
            params.regions = this.regions
            params.only_valid_fide = this.onlyValidByFIDEelo

            if (this.minDate !== '') {
                params.min_date = this.formatDate(this.minDate)
            }
            if (this.maxDate !== '') {
                params.max_date = this.formatDate(this.maxDate)
            }
            if (this.minDays !== '') {
                params.min_days = this.minDays
            }
            if (this.maxDays !== '') {
                params.max_days = this.maxDays
            }

            return params
        }
    },
    watch: {
        displayPerPage() {
            this.currentPage = 1
            this.$fetch()
        },
        sorting() {
            this.currentPage = 1
            this.$fetch()
        }
    },
    methods: {
        prevPage() {
            if (this.currentPage > 0) {
                this.currentPage -= 1
                this.$fetch()
            }
        },
        nextPage() {
            if (this.currentPage <= this.maxPages) {
                this.currentPage += 1
                this.$fetch()
            }
        },
        updatePage(event) {
            const value = event.target.value
            if (parseInt(value) < 1) {
                this.currentPage = 1
            } else if (parseInt(value) > this.maxPages) {
                this.currentPage = this.maxPages
            }
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        updateInput(value) {
            this.searchInput = value
            this.currentPage = 1
            this.$fetch()
        },
        updateMinDate(value) {
            this.minDate = value
            this.currentPage = 1
            this.$fetch()
        },
        updateMaxDate(value) {
            this.maxDate = value
            this.currentPage = 1
            this.$fetch()
        },
        updateMinDays(value) {
            this.minDays = value
            this.currentPage = 1
            this.$fetch()
        },
        updateMaxDays(value) {
            this.maxDays = value
            this.currentPage = 1
            this.$fetch()
        },
        updateRegions(value) {
            this.regions = value
            this.currentPage = 1
            this.$fetch()
        },
        updateValidFIDE(value) {
            this.onlyValidByFIDEelo = value
            this.currentPage = 1
            this.$fetch()
        },
        cleanFilters() {
            this.searchInput = ''
            this.minDate = ''
            this.maxDate = ''
            this.minDays = ''
            this.maxDays = ''
            this.onlyValidByFIDEelo = ''
            this.regions = []
            this.currentPage = 1
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

.header-info {
    margin-top: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 32px !important;
    max-height: 32px !important;
}

.header-sorting {
    display: inline-flex;
}

.sorting {
    margin-left: 10px;
}

.pagination {
    display: flex;
    justify-content: center;
    align-items: center;
}

.pagination input {
    margin-right: 8px;
}

.pagination-btn {
    margin: 4px 16px;
    background-color: var(--color-white);
    border: 1px solid var(--color-border);
    border-radius: var(--border-radius);
    font-weight: bold;
}
.pagination-btn[disabled] {
    opacity: 50%;
}

.pagination-btn img {
    width: 22px;
    padding-top: 2px;
}
.pagination-btn:hover:enabled,
.pagination input:hover {
    border-color: var(--color-border-hover);
    cursor: pointer;
}
</style>
