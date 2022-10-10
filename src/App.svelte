<script lang="ts">
    let numOfLetters = 5;
    let numOfWords = 5;
    let generatedNames = [];
    $: displayedGeneratedNames = generatedNames.join('<br>');

    const vowels = "aeiou".split('');
    const consonants = "bcdfghjklmnpqrstvwxyz".split('');

    function gen() {
        for (let i = 0; i < numOfWords; i++) {
            generate();
        }
    }

    function generate(): void {
        let name = '';
        name += randomLetter(randomBoolean() ? vowels : consonants);

        for (let i = 1; i < numOfLetters; i++) {
            if (consonants.includes(name[i]) && consonants.includes(name[i - 1])) {
                name += randomLetter(vowels);
            } else if(vowels.includes(name[i]) && vowels.includes(name[i - 1])) {
                name += randomLetter(consonants);
            } else {
                name += randomLetter(randomBoolean() ? vowels : consonants);
            }
        }

        generatedNames.push(name);
        generatedNames = generatedNames; // For Svelte to update DOM
        console.log(generatedNames);
    }

    function randomLetter(arr: string[]): string {
        return arr[Math.floor(Math.random() * arr.length)];
    }

    function randomBoolean(): boolean {
        return Math.random() < 0.5;
    }
</script>

<h1>Project Name Generator</h1>
<p>Tries to generate a random name for your project, with a marginal rate of success</p>

<hr>

<label for="num-of-letters">Number of letters per word</label>
<input type="number" min="1" id="num-of-letters" bind:value={numOfLetters}>
<br>
<label for="num-of-words">Number of words to generate</label>
<input type="number" min="1" id="num-of-words" bind:value={numOfWords}>
<br>
<button on:click={gen}>Generate</button>

<p>{@html displayedGeneratedNames}</p>

<style>
    h1 {
        font-size: 2em;
    }

    hr {
        margin-bottom: 5rem;
    }

    input {
        margin-bottom: 1rem;
        width: 3em;
        padding-inline: .8em;
    }
</style>
