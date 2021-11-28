<template>
    <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
    >
        <path
            d="M12.3535 12.8535L10.2322 10.7322"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M19.4247 19.9248L18.0105 18.5106"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M21.8995 16.7427L16.9498 17.4498L16.2427 22.3995L12 18.1569L12.7071 13.2071L17.6569 12.5L21.8995 16.7427Z"
            :stroke="color"
            stroke-width="2"
        />
        <path
            d="M14.8878 8.90503C14.644 8.28273 14.2706 7.69945 13.7678 7.19663C11.8152 5.244 8.64935 5.244 6.69673 7.19663C4.7441 9.14925 4.7441 12.3151 6.69673 14.2677C7.19955 14.7705 7.78283 15.1439 8.40513 15.3877"
            :stroke="color"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
        />
        <path
            d="M18.7632 7.85829C18.3346 6.58283 17.6123 5.38435 16.5963 4.36835C13.0815 0.853632 7.38305 0.853632 3.86833 4.36835C0.353615 7.88307 0.353615 13.5816 3.86833 17.0963C4.88433 18.1123 6.08281 18.8346 7.35827 19.2632"
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
