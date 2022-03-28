<template>
    <main>
        <section v-if="isLoading">
            <div class="progress-roller">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
            </div>
        </section>
        <section class="content_wrap">
            <app-search @find="getMovieDetails"></app-search>
            <app-result :movie="movie" @isLoading="isLoading = false"></app-result>
        </section>
    </main>
</template>

<script>
export default {
    name: 'app-homepage',
    data: function () {
        return {
            movie: '',
            isLoading: false
        }
    },
    methods: {
        async getMovieDetails(movie) {
            this.movie = '';
            this.isLoading = true;
            const response = await fetch(`https://www.omdbapi.com/?apikey=5044463d&t=${movie}`)
            const movies = await response.json()
            return movies.Error ? false : this.movie = movies
        }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

.progress-roller {
    display: block;
    margin: 0 auto;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 80px;
    height: 80px;

    div {
        animation: progress-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
        transform-origin: 40px 40px;

        &:after {
            content: " ";
            display: block;
            position: absolute;
            width: 7px;
            height: 7px;
            border-radius: 50%;
            background: #87BCDE;
            margin: -4px 0 0 -4px;
        }

        &:nth-child(1) {
            animation-delay: -0.036s;

            &:after {
                top: 63px;
                left: 63px;
            }
        }

        &:nth-child(2) {
            animation-delay: -0.072s;

            &:after {
                top: 68px;
                left: 56px;
            }
        }

        &:nth-child(3) {
            animation-delay: -0.108s;

            &:after {
                top: 71px;
                left: 48px;
            }
        }

        &:nth-child(4) {
            animation-delay: -0.144s;

            &:after {
                top: 72px;
                left: 40px;
            }
        }

        &:nth-child(5) {
            animation-delay: -0.18s;

            &:after {
                top: 71px;
                left: 32px;
            }
        }

        &:nth-child(6) {
            animation-delay: -0.216s;

            &:after {
                top: 68px;
                left: 24px;
            }
        }

        &:nth-child(7) {
            animation-delay: -0.252s;

            &:after {
                top: 63px;
                left: 17px;
            }
        }

        &:nth-child(8) {
            animation-delay: -0.288s;

            &:after {
                top: 56px;
                left: 12px;
            }
        }
    }
}

@keyframes progress-roller {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
}

.content_wrap {
    max-width: 1920px;
    margin: 0 auto;
    font-size: 16px;
}
</style>