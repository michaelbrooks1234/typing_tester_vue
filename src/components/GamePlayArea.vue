<script lang="ts">
    import {defineComponent} from 'vue'
    export default defineComponent({
        data() {
            return {
                words: "",
                currentLetter: [0,0] // word, letter
            }
        },
        created() {
            window.addEventListener('keypress', (event) => {
                const element: HTMLElement = document.getElementById("typingarea")!;
                const words = element.children;
                const first = this.currentLetter[0];
                const second = this.currentLetter[1];
                if( event.key == words[first].children[second].innerHTML ){
                        words[first].children[second].className = "green";
                        if(second == words[first].children.length - 1){
                            this.currentLetter[0]++;
                            this.currentLetter[1] = 0;
                            const first = this.currentLetter[0];
                            const second = this.currentLetter[1];
                            words[first].children[second].className += " carrot";
                        }else{
                            this.currentLetter[1]++;
                            const first = this.currentLetter[0];
                            const second = this.currentLetter[1];
                            words[first].children[second].className += " carrot";
                        }
                }else{
                    words[first].children[second].className = "red";
                    words[first].children[second].className += " carrot";
                } 
            })
        },
        methods: {
            async requestWords() {
                const response = await fetch("https://typingbackend.netlify.app/.netlify/functions/getWords", {
                    method: "GET",
                });
                const response_data = await response.json()
                return response_data;
            },
            async createTypingTest (words: any) {
                const word_set = words[0]['words'];
                const typing_test_array: Array<String> = [];
                for(let i = 0; i < 150; i++){
                        const randomElement: String = word_set[Math.floor(Math.random() * word_set.length)];
                        let randomElementSplit: Array<String> = randomElement.split("");
                        for(let j = 0; j < randomElementSplit.length; j++){
                            if(i == 0 && j == 0){
                                randomElementSplit[j] = "<div class='letter carrot'>" + randomElementSplit[j] + "</div>"
                            }else{
                                randomElementSplit[j] = "<div class='letter'>" + randomElementSplit[j] + "</div>"
                            }
                        }
                        if( i != 150 - 1){
                            randomElementSplit.push("<div class='letter'> </div>")
                        }
                        const processedElements = "<div class='word'>" + randomElementSplit.join("") + "</div>";
                        typing_test_array.push(processedElements);
                }

                const typing_test = typing_test_array.join("");
                return typing_test;
            },
            async setup () {
                const typing_test = await this.createTypingTest(await this.requestWords());
                return typing_test;
            }
        },
        computed: {
            setup_words() {
                this.setup().then((test) => {
                    this.words = test;
                })
                return;
            }
        }
    })


</script>

<template>
    <div class="gameplaycontainer">
        {{ setup_words }}
        <div class="typingarea" id="typingarea" v-html="words"> 
        </div>
    </div>
</template>

<style>

    .letter {
        color: white;
    }

    .green {
        color: lime;
        background-color: green;
    }

    .red {
        color: red;
        background-color: pink;
    }

    .carrot {
        border-left: solid 2px black;
    }

    .gameplaycontainer {
        margin: 0;
        padding: 0;
        height: 80vh;
        width: 100vw;
        background-color: gray;
        border-radius: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .typingarea {
        overflow: hidden;
        display: block;
        position: relative;
        height: min-content;
        width: 80vw;
        margin: 1px;
        overflow-wrap: break-word;
    }

    .typingarea div {
        margin: auto;
        position: relative;
        display: inline;
        height: auto;
        width: 1px;
    }

    .typingarea div div {
        font-size: 24px;
        margin: auto;
        position: relative;
        display: inline;
        height: auto;
        width: 1px;
    }

</style>