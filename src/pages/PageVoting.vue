<template>
    <div>
        <h1>
            Voting
        </h1>

        <ul>
            <li v-for="song in mappedSongs" :key="song.id">
                {{song.artistObject.name}} - {{ song.title }}
            </li>

        </ul>
    </div>
</template>

<script>
    export default {
        name: "PageVoting",
        mounted() {
            this.fetchSongs();
        },
        data() {
            return {
                songs: [],
                artists: [],
                mappedSongs: []
            }
        },
        methods: {
            fetchSongs() {
                fetch("http://webservies.be/eurosong/Songs", {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json"
                    },
                })
                .then((response) => response.json())
                .then((songs) => {
                    console.log(songs);
                    this.songs = songs;
                    this.fetchArtists();
                });
            },
            fetchArtists() {
                fetch("http://webservies.be/eurosong/Artists", {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json"
                    },
                })
                .then((response) => response.json())
                .then((artists) => {
                    this.artists = artists;
                    this.mixSongs();
                });
            },
            mixSongs() {
                this.mappedSongs = this.songs.map((song)=> {
                    song.artistObject = this.artists.find((artist) => {
                        return song.artist == artist.id;
                    });
                    return song;
                });

                console.log(this.mappedSongs);
            }
        }
    }
</script>
