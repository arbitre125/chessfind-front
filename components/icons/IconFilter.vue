<template>
    <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
    >
        <path
            d="M3 17H13"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
        />
        <path
            d="M10 7H20"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
        />
        <circle cx="17" cy="17" r="3" :stroke="color" stroke-width="2" />
        <circle cx="6" cy="7" r="3" :stroke="color" stroke-width="2" />
    </svg>
</template>

<script>
export default {
    props: {
        color: {
            type: String,
            default: '#5a5a5a'
        }
    }
}
</script>
