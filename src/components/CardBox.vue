<template>
    <div class="card-box" v-if="isLoading">
        <div class="loader"></div>
    </div>
    <div class="card-box" v-else>
        <h1>Advice #{{ id }}</h1>
        <blockquote>{{ advice  }}</blockquote>
        <picture>
            <source media="(min-width: 768px)" srcset="/images/pattern-divider-desktop.svg">
            <img src="/images/pattern-divider-mobile.svg" alt="Divider">
        </picture>
        <button @click="getAdvice">
            <img src="/images/icon-dice.svg" alt="Dice">
        </button>
    </div>
</template>

<script lang="ts">
export default {
    name: 'CardBox',
    data() {
        return {
            advice: '',
            id: 0,
            isLoading: false,
        };
    },
    async mounted() {
        await this.getAdvice();
    },
    methods: {
        async getAdvice() {
            this.isLoading = true;
            const response = await fetch('https://api.adviceslip.com/advice');
            if (!response.ok) {
                throw new Error(response.statusText);
            }
            const { slip: { advice, id } } = await response.json();

            this.id = id;
            this.advice = advice;
            this.isLoading = false;
        }
    }
}

</script>

<style scoped>
.card-box {
    background-color: var(--dark-grayish-blue);
    border-radius: 1rem;
    padding: 4rem;
    box-shadow: 0 0.5rem 0.5rem var(--dark-blue);

    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;

    max-width: 40rem;

    position: relative;
}

h1 {
    font-size: 1rem;
    color: var(--neon-green);
    text-align: center;
    margin-bottom: 1rem;
    text-transform: uppercase;
    letter-spacing: 0.25rem;
}

blockquote {
    color: var(--light-cyan);
    font-size: 2rem;
    text-align: center;
    line-height: 1.5;
    quotes: "“" "”";
}

blockquote:before {
  content: open-quote;
}

blockquote:after {
  content: close-quote;
}

picture {
    width: 100%;
    margin: 1rem 0;
}

picture > img {
    width: 100%;
}

button {
    background-color: var(--neon-green);
    border-radius: 99px;
    border: none;
    padding: 1.5rem;
    cursor: pointer;

    position: absolute;
    bottom: -2.5rem;
    box-shadow: 0 0.5rem 0.5rem var(--dark-blue);

    transition: all 0.3s ease;
}

button:hover {
    box-shadow: 0 0 1.5rem var(--neon-green);
}

button > img {
    width: 2rem;
    height: 2rem;
}

.loader {
    border: 0.5rem solid var(--light-grayish-blue);
    border-top: 0.5rem solid var(--neon-green);
    border-radius: 50%;
    width: 3rem;
    height: 3rem;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}
</style>