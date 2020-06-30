<script>
    import Question from './Question.svelte'

    const getQuiz = async() => {
        const res = await fetch('https://opentdb.com/api.php?amount=10')
        const quiz = await res.json()
        return quiz
    }

    const nextQuestion = () => {
        activeQuestion = activeQuestion + 1
    }

    const resetQuiz = () => {
        score = 0
        quiz = getQuiz()
    }

    const addToScore = () => {
        score = score + 1
    }

    let quiz = getQuiz(),
        activeQuestion = 0,
        score = 0
</script>

<div>
    <button on:click={resetQuiz}>Start New Quiz</button>

    <h3>Your score is {score}</h3>
    <h4>Question #{activeQuestion + 1}</h4>

    {#await quiz}
        Loading ...
    {:then data}
        {#each data.results as question, index}
            {#if index === activeQuestion}
                <Question {addToScore} {nextQuestion} {question} />
            {/if}
        {/each}
    {:catch error}
        <!-- promise was rejected -->
    {/await}
</div>