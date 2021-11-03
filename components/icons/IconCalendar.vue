<template>
    <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
    >
        <rect
            x="2"
            y="3"
            width="20"
            height="20"
            rx="3"
            :stroke="color"
            stroke-width="2"
        />
        <path
            d="M7 1V3"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M17 1V3"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M2 8H22"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M7.5 13H9.5"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M14.5 13H16.5"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M7.5 17H9.5"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M14.5 17H16.5"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
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
