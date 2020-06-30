<script>
    import { fly } from 'svelte/transition'
    import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte'
    import Question from './Question.svelte'
    import Modal from './Modal.svelte'

    onMount(() => {
        console.log("I mounted")
    })

    beforeUpdate(() => {
        console.log("before update")
    })

    afterUpdate(() => {
        console.log("after update")
    })

    const getQuiz = async() => {
        const res = await fetch('https://opentdb.com/api.php?amount=10')
        const quiz = await res.json()
        return quiz
    }

    const nextQuestion = () => {
        activeQuestion = activeQuestion + 1
    }

    const resetQuiz = () => {
        isModalOpen = false
        score = 0
        activeQuestion = 0
        quiz = getQuiz()
    }

    const addToScore = () => {
        score = score + 1
    }

    // Reactive Statement
    $: if (score > 0) {
        isModalOpen = true
    }

    $: questionNumber = activeQuestion + 1

    let quiz = getQuiz(),
        activeQuestion = 0,
        score = 0,
        isModalOpen = false
</script>

<style>
    .fade-wrapper {
        position: absolute;
    }
</style>

<div>
    <button on:click={resetQuiz}>Start New Quiz</button>

    <h3>Your score is {score}</h3>
    <h4>Question #{questionNumber}</h4>

    {#await quiz}
        Loading ...
    {:then data}
        {#each data.results as question, index}
            {#if index === activeQuestion}
                <div in:fly={{x: 100}} out:fly={{x: -200}} class="fade-wrapper">
                    <Question {addToScore} {nextQuestion} {question} />
                </div>
            {/if}
        {/each}
    {:catch error}
        <!-- promise was rejected -->
    {/await}
</div>

{#if isModalOpen}
    <Modal on:close={resetQuiz}>
        <h2>You won!</h2>
        <p>Congratulations</p>
        <button on:click={resetQuiz}>Start Over</button>
    </Modal>
{/if}