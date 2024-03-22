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

                <!-- // v-if="artist.name == null" -->
                <button  @click="deleteArtist(artist.id)">
                    delete
                </button>
            </li>
        </ul>

        <hr>
        <input type="text" v-model="newArtist">
        <button @click="addArtist()"> Add artist</button>
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
                artists: [],
                newArtist: ""
            }
        },
        methods: {
            addArtist() {
                fetch("http://webservies.be/eurosong/Artists", {
                    method: "POST",
                    headers: {
                        'accept': 'application/json',
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({ name: this.newArtist })
                })
                .then((response) => response.json())
                .then(() => {
                    this.fetchArtists();
                });
            },
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
