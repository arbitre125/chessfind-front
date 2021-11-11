<template>
    <div class="tournament-card">
        <a
            :href="tournament.link"
            target="_blank"
            class="tournament-card-header"
        >
            <div class="tournament-name">
                {{ tournament.name }}
            </div>
        </a>
        <div class="tournament-card-body">
            <span v-if="isFedSupported" class="tournament-country">
                <img :src="getFlag" class="tournament-flag" />
                {{ $t(`region.${tournament.fed.toLowerCase()}`) }}
            </span>
            <span v-if="getCity" class="tournament-city">
                <IconLocation class="icon" />
                <a :href="getCityLink" target="_blank">
                    {{ getCity }}
                </a>
            </span>
            <span class="tournament-dates">
                <IconCalendar class="icon" />
                {{ tournament.start }} - {{ tournament.end }}
            </span>
            <span class="tournament-source">
                <IconLink class="icon" />
                {{ tournament.source }}
            </span>
        </div>
    </div>
</template>

<script>
import { getCountries } from '../utils/filters'
import IconLocation from './icons/IconLocation'
import IconCalendar from './icons/IconCalendar'
import IconLink from './icons/IconLink'

export default {
    components: {
        IconLocation,
        IconCalendar,
        IconLink
    },
    props: {
        tournament: {
            type: Object,
            default: () => {}
        }
    },
    computed: {
        isFedSupported() {
            const fed = this.tournament.fed.toUpperCase()
            return getCountries().includes(fed)
        },
        getFlag() {
            return require(`@/assets/flags/${this.tournament.fed.toLowerCase()}.svg`)
        },
        getCity() {
            if (this.isFedSupported & !this.tournament.city) {
                return this.$t(`region.${this.tournament.fed.toLowerCase()}`)
            }
            return this.tournament.city
        },
        getCityLink() {
            const city = this.tournament.city.toLowerCase()
            const URLcontain = ['http', '.c', '.d', '.e', '.o']
            const isUrl = URLcontain.some((str) => {
                return city.includes(str)
            })
            if (isUrl) {
                if (!city.includes('http')) {
                    return 'http://' + city
                }
                return city
            }
            const url = 'https://www.google.com/maps/search/'
            const country = this.$t(
                `region.${this.tournament.fed.toLowerCase()}`
            )
            const link = city ? url + city + ',' + country : url + country

            return link
        }
    }
}
</script>

<style scoped>
.icon {
    width: 18px;
    height: 14px;
    margin-right: -2px;
}

.tournament-card {
    margin: 8px 0;
    padding: 12px 16px 16px 16px;
    border: 1px solid var(--color-border);
    border-radius: var(--border-radius);
}

.tournament-card:hover {
    border-color: var(--color-border-hover);
}

.tournament-card-header {
    color: var(--color-black-dark);
}

.tournament-card-body {
    color: var(--color-black-light);
    display: flex;
    flex-wrap: wrap;
}

.tournament-card-body > span {
    margin: 4px 40px 0 0;
}

@media only screen and (max-width: 768px) {
    .tournament-card-body > span {
        flex-direction: column;
        align-content: center;
        align-items: flex-start;
        justify-content: flex-start;
        margin-top: 8px;
    }
}

.tournament-card > a {
    text-decoration: none;
}

.tournament-name {
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 8px;
}

.tournament-country,
.tournament-city,
.tournament-dates {
    margin-right: 40px;
    min-width: 120px;
}

.tournament-city > a {
    color: var(--color-text-body);
}

.tournament-flag {
    width: 18px;
    height: 14px;
    border-radius: 2px;
    margin-right: 4px;
    margin-bottom: -2px;
    box-shadow: var(--color-shadow);
}

.tournament-dates {
    margin-right: 40px;
    display: inline-block;
}

.tournament-source {
    display: inline-block;
}
</style>
