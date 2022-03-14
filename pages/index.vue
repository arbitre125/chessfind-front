<template>
    <div>
        <Navbar :menu="false" />
        <div v-if="error" class="message error">
            <img src="@/assets/icons/error.svg" class="placeholder-icon" />
            <h2>{{ $t('error.error_found') }}</h2>
        </div>
        <div v-else class="content-container">
            <div class="hero">
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
            </div>
            <div v-if="loading">
                <LandingSkeleton />
            </div>
            <div v-else class="landing-content">
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
                        <h6>
                            {{ time.count }} {{ $t('results').toLowerCase() }}
                        </h6>
                    </nuxt-link>
                </section>
                <section class="popular-time-control flex-grid">
                    <nuxt-link
                        v-for="time in popularTimeControls.slice(
                            0,
                            maxDisplayTimeControls
                        )"
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
                    <button
                        v-if="popularTimeControls.length > 5"
                        class="secondary-btn"
                        @click="showAllTimeControls = !showAllTimeControls"
                    >
                        <span v-if="showAllTimeControls">{{
                            $t('action.display_less')
                        }}</span>
                        <span v-else>{{ $t('action.display_all') }}</span>
                    </button>
                </section>
                <section class="time-control popular-time-control flex-grid">
                    <h2>{{ $t('play_in_fav.region') }}</h2>
                    <nuxt-link
                        v-for="region in popularRegions.slice(
                            0,
                            maxDisplayRegions
                        )"
                        :key="region"
                        :to="{
                            name: 'tournaments',
                            params: { regions: [region.toUpperCase()] }
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
                    <button
                        v-if="popularRegions.length > 5"
                        class="secondary-btn"
                        @click="showAllRegions = !showAllRegions"
                    >
                        <span v-if="showAllRegions">{{
                            $t('action.display_less')
                        }}</span>
                        <span v-else>{{ $t('action.display_all') }}</span>
                    </button>
                </section>
                <section class="time-control popular-time-control flex-grid">
                    <h2>{{ $t('play_in_fav.city') }}</h2>
                    <nuxt-link
                        v-for="city in popularCities.slice(0, maxDisplayCities)"
                        :key="city"
                        :to="{
                            name: 'tournaments',
                            params: { city: city }
                        }"
                        class="col card-city"
                    >
                        <h3>{{ $t(`cities.${city.replace(' ', '_')}`) }}</h3>
                        <div class="layer"></div>
                        <img
                            :src="
                                require(`@/assets/cities/${city.replace(
                                    ' ',
                                    '_'
                                )}.png`)
                            "
                        />
                    </nuxt-link>
                    <button
                        v-if="popularCities.length > 5"
                        class="secondary-btn"
                        @click="showAllCities = !showAllCities"
                    >
                        <span v-if="showAllCities">{{
                            $t('action.display_less')
                        }}</span>
                        <span v-else>{{ $t('action.display_all') }}</span>
                    </button>
                </section>
            </div>
        </div>
        <Footer />
    </div>
</template>

<script>
import { getCountries } from '../utils/filters'
import LandingSkeleton from '../components/LandingSkelete.vue'

export default {
    components: { LandingSkeleton },
    data() {
        return {
            loading: false,
            error: false,
            showAllTimeControls: false,
            showAllRegions: false,
            showAllCities: false,
            searchInput: '',
            minDate: '',
            maxDate: '',
            timeControlType: '',
            timeControls: [],
            popularTimeControls: [],
            popularRegions: [],
            popularCities: []
        }
    },
    async fetch() {
        this.loading = true

        await this.$axios
            .post(this.$config.apiURL + '/', this.getParams)
            .then((response) => {
                this.assignTimeControlsTypes(response.data.time_control_types)
                this.assignTimeControlsValues(response.data.time_control_values)
                this.assignRegions(response.data.regions)
                this.assignCities(response.data.cities)
                this.loading = false
            })
            .catch(() => {
                this.loading = false
                this.error = true
            })
    },
    computed: {
        today() {
            return new Date().toISOString().split('T')[0]
        },
        maxDisplayTimeControls() {
            return this.showAllTimeControls
                ? this.popularTimeControls.length
                : 5
        },
        maxDisplayRegions() {
            return this.showAllRegions ? this.popularRegions.length : 5
        },
        maxDisplayCities() {
            return this.showAllCities ? this.popularCities.length : 5
        }
    },
    methods: {
        isFedSupported(region) {
            const fed = region.toUpperCase()
            return getCountries().includes(fed)
        },
        assignTimeControlsTypes(value) {
            for (const time of value) {
                this.timeControls.push({
                    display: time[0],
                    count: time[1],
                    url: time[0] === 'all' ? '' : time[0]
                })
            }
        },
        assignTimeControlsValues(value) {
            for (const time of value) {
                const s = time[0].split('+')
                this.popularTimeControls.push({
                    min: s[0].replace(/[^\d.]/g, ''),
                    sec: s[1].replace(/[^\d.]/g, '')
                })
            }
        },
        assignRegions(value) {
            for (const region of value) {
                if (this.isFedSupported(region[0])) {
                    this.popularRegions.push(region[0].toLowerCase())
                }
            }
        },
        assignCities(value) {
            for (const city of value) {
                this.popularCities.push(city[0].toLowerCase())
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

.hero {
    margin-bottom: 80px;
}

section {
    margin: 20px 0;
}
section.time-control {
    margin-top: 40px;
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

.card-city {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    align-content: flex-start;
    min-width: 18%;
    max-width: 20%;
    height: 180px;
    border: 1px solid rgba(38, 0, 192, 0.25);
    color: var(--color-primary);
    box-sizing: border-box;
    border-radius: 8px;
    text-decoration: none;
    flex-grow: 1;
}
.card-city:hover {
    border-color: var(--color-primary);
}

.popular-time-control {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
}

.popular-time-control h2 {
    margin-bottom: 8px;
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

.secondary-btn {
    width: 100%;
}

.card-city h3 {
    position: absolute;
    top: 0;
    margin: 0;
    border-radius: 8px;
    padding-top: 12px;
    text-align: center;
    font-weight: bold;
    width: 100%;
    height: 100%;
    background: linear-gradient(
        180deg,
        #ffffff 0%,
        rgba(255, 255, 255, 0.8) 20.31%,
        rgba(255, 255, 255, 0.7) 23.7%,
        rgba(255, 255, 255, 0) 50%
    );
}

.card-city img {
    width: 100%;
    height: 100%;
    background-size: cover;
}
</style>
