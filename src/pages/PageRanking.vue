<template>
    <div>
        <h1>
            Ranking
        </h1>

        <div>
            <div v-for="song in mappedSongs" :key="song.id">
                {{song.artistObject.name}} - {{ song.title }} - {{ song.points }}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "PageRanking",
        mounted() {
            this.fetchSongs();
        },
        data() {
            return {
                songs: [],
                artists: [],
                votes: [],
                mappedSongs: [],
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
                    this.fetchVotes();
                });
            },
            fetchVotes() {
                fetch("http://webservies.be/eurosong/Votes", {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json"
                    },
                })
                .then((response) => response.json())
                .then((votes) => {
                    this.votes = votes;
                    this.mixSongs();
                });
            },
            mixSongs() {
                this.mappedSongs = this.songs.map((song)=> {
                    song.artistObject = this.artists.find((artist) => {
                        return song.artist == artist.id;
                    });

                    let points = 0;
                    this.votes.forEach((vote) => {
                        if (vote.songID == song.id) {
                            points += vote.points;
                        }
                    });

                    song.points = points;

                    return song;
                });
                
                this.mappedSongs.sort((a, b) => b.points - a.points);
            }
        }
    }
</script>
