<template>
    <div>
        <h1>
            Voting
        </h1>

        <div>
            <div v-for="(song, index) in mappedSongs" :key="song.id">
                <div v-if="index == activeSongIndex">
                    {{song.artistObject.name}} - {{ song.title }}
                </div>
            </div>
        </div>
        <button @click="prevSong()" :disabled="activeSongIndex == 0">
            Prev
        </button>
        <button @click="nextSong()" v-bind:disabled="activeSongIndex == (mappedSongs.length - 1)">
            Next
        </button>
        <br>
        <div v-for="(button, index) in buttons" :key="index">
            <button @click="vote(index, button.points)" :disabled="button.isVoted == true">
                Add {{ button.points }} {{ button.isVoted }}
            </button>
        </div>

        <button @click="changePage()">
            Go to ranking page
        </button>
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
                mappedSongs: [],
                activeSongIndex: 0,
                buttons: [
                    {
                        points: 2,
                        isVoted: false,
                    },
                    {
                        points: 4,
                        isVoted: false,
                    },
                    {
                        points: 6,
                        isVoted: false,
                    },
                ]
            }
        },
        methods: {
            changePage() {
                this.$emit("changeActivePage", 'ranking');
            },
            vote(buttonIndex, amountOfPoints) {
                let songId = this.mappedSongs[this.activeSongIndex].id;
                this.buttons[buttonIndex].isVoted = true;

                if (this.buttons.filter((button) => button.isVoted == false).length == 0) {
                    setTimeout(() => {
                        this.$emit("changeActivePage", 'ranking');
                    }, 1000)
                }

                fetch("http://webservies.be/eurosong/Votes", {
                    method: "POST",
                    headers: {
                        'accept': 'application/json',
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({ 
                        songID: songId,
                        ip:"undefined",
                        points: amountOfPoints
                    })
                })
                .then((response) => response.json())
                .then((vote) => {
                    console.log(vote);
                });
            },
            prevSong(){
                this.activeSongIndex--;
            },
            nextSong() {
                this.activeSongIndex++;
            },
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
