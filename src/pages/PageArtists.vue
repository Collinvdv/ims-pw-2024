<template>
    <div>
        <h1>
            Artists
        </h1>

        <button @click="fetchArtists()">
            Fetch all artists
        </button>

        <ul>
            <li v-for="artist in artists" :key="artist.id">
                {{ artist.name }} {{ artist.id }}

                <button v-if="artist.name == null" @click="deleteArtist(artist.id)">
                    delete
                </button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "PageArtists",
        mounted() {
            this.fetchArtists();
        },
        data() {
            return {
                artists: []
            }
        },
        methods: {
            deleteArtist(id) {
                fetch("http://webservies.be/eurosong/Artists?id=" + id, {
                    method: "DELETE",
                    headers: {
                        "Content-Type": "application/json"
                    },
                })
                .then((response) => response)
                .then(() => {
                    this.fetchArtists();
                })
            },
            fetchArtists() {
                fetch("http://webservies.be/eurosong/Artists", {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json"
                    },
                })
                .then((response) => response.json())
                .then((data) => {
                    this.artists = data;
                });
            }
        }
    }
</script>
