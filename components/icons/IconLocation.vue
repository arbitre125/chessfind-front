<template>
    <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
    >
        <path
            d="M21 11C21 15.2404 15.6944 20.3382 13.2304 22.477C12.5222 23.0917 11.4915 23.0742 10.802 22.4385C8.3525 20.1805 3 14.7591 3 10.5C3 4.97715 6.47715 1 12 1C17.5228 1 21 5.47715 21 11Z"
            :stroke="color"
            stroke-width="2"
        />
        <circle cx="12" cy="10" r="3" :stroke="color" stroke-width="2" />
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
