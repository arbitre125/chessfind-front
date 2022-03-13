<template>
    <div>
        <Navbar :menu="false" />
        <div class="content-container">
            <h1>{{ $t('title') }}</h1>
            <div class="flex-grid search-box">
                <input
                    v-model="searchInput"
                    type="text"
                    :placeholder="$t('action.search') + '...'"
                />
            </div>
            <div class="flex-grid search-box btn">
                <div class="filter-item col">
                    <label>
                        {{ $t('filter.min_date') }}
                        <input v-model="minDate" type="date" :min="today" />
                    </label>
                </div>
                <div class="filter-item col">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input
                            v-model="maxDate"
                            type="date"
                            :min="minDate || today"
                        />
                    </label>
                </div>
                <div class="filter-item col">
                    <label>
                        {{ $t('filter.time_control') }}
                        <select v-model="timeControlType">
                            <option value="" selected>
                                {{ $t('action.select_option') }}
                            </option>
                            <option value="standard">
                                {{ $t('time_control.standard') }}
                            </option>
                            <option value="rapid">
                                {{ $t('time_control.rapid') }}
                            </option>
                            <option value="blitz">
                                {{ $t('time_control.blitz') }}
                            </option>
                        </select>
                    </label>
                </div>
                <div class="filter-item col search-btn">
                    <button @click="searchTournaments">
                        {{ $t('action.search') }}
                    </button>
                </div>
            </div>
            <section class="time-control flex-grid">
                <h2>{{ $t('play_in_fav.time_control') }}</h2>
                <nuxt-link
                    v-for="time in timeControls"
                    :key="time.display"
                    :to="{
                        name: 'tournaments',
                        params: { time_control_type: time.url }
                    }"
                    class="col card-time-control"
                >
                    <h3>{{ $t(`time_control.${time.display}`) }}</h3>
                    <h6>{{ time.count }} {{ $t('results').toLowerCase() }}</h6>
                </nuxt-link>
            </section>
            <section class="popular-time-control flex-grid">
                <nuxt-link
                    v-for="time in popularTimeControls"
                    :key="time.min + ' ' + time.sec"
                    :to="{
                        name: 'tournaments',
                        params: {
                            time_control_min: time.min,
                            time_control_sec: time.sec
                        }
                    }"
                    class="col chip-time-control"
                >
                    {{
                        time.min +
                        ' ' +
                        $t('min').toLowerCase() +
                        ' + ' +
                        time.sec +
                        ' ' +
                        $t('sec').toLowerCase()
                    }}
                </nuxt-link>
            </section>
            <!--
            <section class="time-control flex-grid">
                <h2>{{ $t('play_in_fav.region') }}</h2>
                <nuxt-link
                    v-for="continent in continents"
                    :key="continent"
                    :to="{
                        name: 'tournaments',
                        params: { continent: continent }
                    }"
                    class="col card-time-control"
                >
                    {{ $t(`region.${continent}`) }}
                </nuxt-link>
            </section>
            <section class="popular-time-control flex-grid">
                <nuxt-link
                    v-for="region in popularRegions"
                    :key="region"
                    :to="{
                        name: 'tournaments',
                        params: { regions: ['CAT'] }
                    }"
                    class="col chip-time-control"
                >
                    <img
                        :src="
                            require(`@/assets/flags/${region.toLowerCase()}.svg`)
                        "
                        class="tournament-flag"
                    />
                    {{ $t(`region.${region}`) }}
                </nuxt-link>
            </section>
            -->
        </div>
        <Footer />
    </div>
</template>

<script>
export default {
    data() {
        return {
            searchInput: '',
            minDate: '',
            maxDate: '',
            timeControlType: '',
            timeControls: []
        }
    },
    async fetch() {
        await this.$axios
            .post(this.$config.apiURL + '/', this.getParams)
            .then((response) => {
                this.assignTimeControls(response.data.time_control_types)
            })
            .catch(() => {
                this.error = true
            })
    },
    computed: {
        today() {
            return new Date().toISOString().split('T')[0]
        },
        popularTimeControls() {
            return [
                { min: 90, sec: 30 },
                { min: 3, sec: 2 },
                { min: 15, sec: 5 },
                { min: 10, sec: 0 },
                { min: 60, sec: 0 }
            ]
        },
        continents() {
            return ['eu', 'na', 'sa', 'as', 'af', 'oc']
        },
        popularRegions() {
            return [
                'esp',
                'cat',
                'and',
                'por',
                'fra',
                'ger',
                'ita',
                'eng',
                'gre',
                'sui'
            ]
        }
    },
    methods: {
        assignTimeControls(value) {
            for (const time of value) {
                this.timeControls.push({
                    display: time[0],
                    count: time[1],
                    url: time[0] === 'all' ? '' : time[0]
                })
            }
        },
        searchTournaments() {
            this.$router.push({
                name: 'tournaments',
                params: {
                    search_input: this.searchInput,
                    min_date: this.minDate,
                    max_date: this.maxDate,
                    time_control_type: this.timeControlType
                }
            })
        }
    }
}
</script>

<style scoped>
h1,
h2 {
    width: 100%;
}
h1 {
    font-size: 32px;
    margin: 32px 8px;
    text-align: center;
    color: var(--color-primary);
}
h3,
h6 {
    font-weight: 500;
    margin: 2px 0;
}

.content-container {
    min-height: calc(100vh - 130px);
}

.flex-grid {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    margin: 0 auto;
    gap: 16px;
    margin-bottom: 20px;
}

.col {
    flex: 1;
}

.search-box {
    width: 800px;
    max-width: 100%;
}

.search-box input {
    max-width: 600px;
}

@media only screen and (max-width: 768px) {
    .search-box.btn {
        flex-direction: column;
    }
}

.search-btn {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
}

section {
    margin: 20px 0;
}
section.time-control {
    margin-top: 60px;
}

.card-time-control,
.chip-time-control {
    padding: 8px 12px;
    background: rgba(38, 0, 192, 0.05);
    border: 1px solid rgba(38, 0, 192, 0.25);
    box-sizing: border-box;
    border-radius: 8px;
    color: var(--color-primary);
    text-decoration: none;
    text-align: center;
}

.card-time-control {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 280px;
    height: 80px;
    font-size: 18px;
}
.card-time-control:hover,
.chip-time-control:hover {
    border-color: var(--color-primary);
}

.popular-time-control {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
}

.chip-time-control {
    font-size: 16px;
    min-width: 160px;
}

.chip-time-control:nth-child(3n) {
    page-break-after: always; /* CSS 2.1 syntax */
    break-after: always; /* CSS 3 syntax */
}

input,
select {
    width: 100%;
}
</style>
