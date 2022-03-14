<template>
    <div class="home">
        <div class="feature-card">
            <router-link to="/movie/tt040959">
                <img src="https://techkalzen.com/wp-content/uploads/2020/01/anime-naruto-shippuden-duoc-xem-nhieu-nhat-trong-thap-ki-2.jpg" alt="Naruto" class="feature-img" />
                <div class="detail">
                    <h3>Naruto</h3>
                    <p>
                        Naruto Shippuden is by far my most favourite Anime Series. Even though it has a lot of fillers, I would say it was too good thanks to the protagonist Naruto Uzumaki who has
                        become a hero for me. Just like how Zabuza told in one of the initial episodes ''Narutos words are sharper than any blade''. His words about his everlasting dream of becoming
                        the best has somehow impacted me. The portrayal of each and every character has been spectacular. Be it the action scenes or be it Narutos interaction with his friends
                        everything has been dealt with nicely. Just to see Naruto become a person who was hated by his villagers to him becoming a Shinobi respected throughout the world is extremely
                        motivating.
                    </p>
                </div>
            </router-link>
        </div>
        <form v-on:submit.prevent="SearchMovies()" class="search-box">
            <input type="text" placeholder="What are you looking for?" v-model="search" />
            <input type="submit" value="Search" />
        </form>
        <div class="movies-list">
            <div class="movie" v-for="movie in movies" v-bind:key="movie.imdbID">
                <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
                    <div class="product-image">
                        <img :src="movie.Poster" alt="Movie Poster" />
                        <div class="type">{{ movie.Type }}</div>
                    </div>
                    <div class="detail">
                        <div class="year">{{ movie.Year }}</div>
                        <h3>{{ movie.Title }}</h3>
                    </div>
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from "vue";
import env from "../env.js";
export default {
    setup() {
        const search = ref("");
        const movies = ref([]);

        const SearchMovies = () => {
            if (search.value != "") {
                fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
                    .then((res) => res.json())
                    .then((data) => {
                        movies.value = data.Search;
                        search.value = "";
                    });
            }
        };

        return {
            search,
            movies,
            SearchMovies,
        };
    },
};
</script>

<style lang="scss">
.home {
    .feature-card {
        position: relative;
        .feature-img {
            display: block;
            width: 100%;
            height: 300px;
            object-fit: cover;
            position: relative;
            z-index: 0;
        }

        .detail {
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.6);
            padding: 16px;
            z-index: 1;
            h3 {
                color: #fff;
                margin-bottom: 16px;
            }
            p {
                color: #fff;
            }
        }
    }

    .search-box {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 16px;
        input {
            display: block;
            appearance: none;
            border: none;
            outline: none;
            background: none;

            &[type="text"] {
                width: 100%;
                color: #fff;
                background-color: #496583;
                font-size: 20px;
                padding: 10px 16px;
                border-radius: 8px;
                margin-bottom: 15px;
                transition: all 0.4s ease;

                &::placeholder {
                    color: #f3f3f3;
                }

                &:focus {
                    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
                }
            }

            &[type="submit"] {
                width: 100%;
                max-width: 300px;
                background-color: #42b883;
                padding: 16px;
                border-radius: 8px;
                color: #fff;
                font-size: 20px;
                text-transform: uppercase;
                transition: all 0.4s ease;
            }

            &:active {
                background-color: #3b8070;
            }
        }
    }

    .movies-list {
        display: flex;
        flex-wrap: wrap;
        margin: 0px 8px;
        .movie {
            max-width: 50%;
            flex: 1 1 50%;
            padding: 16px 8px;
            .movie-link {
                display: flex;
                flex-direction: column;
                height: 100%;
                .product-image {
                    position: relative;
                    display: block;
                    img {
                        display: block;
                        width: 100%;
                        height: 275px;
                        object-fit: cover;
                    }

                    .type {
                        position: absolute;
                        padding: 8px 16px;
                        background-color: #42b883;
                        color: #fff;
                        bottom: 16px;
                        left: 0;
                        text-transform: capitalize;
                    }
                }

                .detail {
                    background-color: #496583;
                    padding: 16px 8px;
                    flex: 1 1 100%;
                    border-radius: 0 0 8px 8px;

                    .year {
                        color: #aaa;
                        font-size: 14px;
                    }

                    h3 {
                        color: #fff;
                        font-weight: 600;
                        font-size: 18px;
                    }
                }
            }
        }
    }
}
</style>
