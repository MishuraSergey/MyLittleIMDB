<template>
    <nav class="search_wrap">
        <div class="input_wrap">
            <input class="input_field" type="text" placeholder="Enter the movie title..." v-model="userInput"
                   @keyup="titleSearch" @keydown.down.stop.prevent="focusOnList">
            <div class="search_result">
                <button class="input_btn" v-for="(movie,index) in movies"
                        :tabindex="index"
                        :key="index"
                        @click="getMovieDetails"
                        @keydown.up.down.stop.prevent="selectPrevNextFromList">{{movie.Title}}
                </button>
            </div>
        </div>
    </nav>
</template>

<script>
export default {
    name: 'app-search',
    data: function () {
        return {
            userInput: '',
            movies: []
        }
    },
    methods: {
        async titleSearch() {
            this.movies = [];
            const response = await fetch(`https://www.omdbapi.com/?apikey=5044463d&s=${this.userInput}`),
                  movies = await response.json();
            return movies.Response ? this.movies = movies.Search : false
        },
        getMovieDetails(e) {
            this.userInput = e.target.innerText
            this.movies = []
            return this.$emit('find', e.target.innerText)
        },
        focusOnList() {
            const btn = document.querySelector('.search_result').firstElementChild;
            if (btn){
                btn.classList.add('focused');
                return btn.focus();
            }
        },
        selectPrevNextFromList(e){
            let movieTitle = {
                el: e.target,
                nextEl: e.target.nextElementSibling,
                prevEl: e.target.previousElementSibling,
                firstTitle: document.querySelector('.search_result').firstElementChild,
                lastTitle: document.querySelector('.search_result').lastElementChild,
                switchFocus: function(elem){
                    this.el.classList.remove('focused');
                    elem.focus();
                    elem.classList.add('focused');
                },
                selectNext: function() {
                    return this.nextEl ? this.switchFocus(this.nextEl) : this.switchFocus(this.firstTitle);
                },
                selectPrev: function(){
                    return this.prevEl ? this.switchFocus(this.prevEl) : this.switchFocus(this.lastTitle);
                }
            }
            return e.keyCode === 40 ? movieTitle.selectNext() : movieTitle.selectPrev();
        }
    }
}
</script>

<style scoped lang="scss">
.search_wrap {
    background-color: #87BCDE;
    position: relative;
    padding-top: 59px;

    .input_wrap {
        width: 300px;
        margin: 0 auto;
        position: absolute;
        top: 10px;
        left: 50%;
        transform: translateX(-50%);

        .input_field {
            border: .1em solid #243B4A;
            padding: .5em;
            font-size: 1em;
            display: block;
            background-color: #fff;
            text-align: left;
            width: 100%;
            box-sizing: border-box;
            color: #243B4A;
        }

        .input_btn {
            @extend .input_field;
            border-top: none;
            border-bottom: none;
            width: 100%;
            background: #fff;

            &:last-of-type {
                border-bottom: .1em solid #000;
            }

            &:hover, &:focus {
                background-color: #d2d2d2;
                transition: background-color .1s ease-out;
                cursor: pointer;
                outline: none;
            }
        }
    }
}

</style>
