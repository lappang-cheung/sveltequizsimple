<script>

    import Question from './Question.svelte'

    const getQuiz = async() => {
        const res = await fetch('https://opentdb.com/api.php?amount=10')
        const quiz = await res.json()
        return quiz
    }

    const handleClick = () => {
        quiz = getQuiz()
    }

    let quiz = getQuiz()
</script>

<div>
    <button on:click={handleClick}>Get New Questions</button>

    {#await quiz}
        Loading ...
    {:then data}
        {#each data.results as question}
            <Question {question} />
        {/each}
    {:catch error}
        <!-- promise was rejected -->
    {/await}
</div>