<script>
    export let question 

    let answers = question.incorrect_answers
        .map(answer => {
            return {
                answer,
                correct: false
            }
        }),

        allAnswers = [
            ...answers, 
            {
                answer: question.correct_answer,
                correct: true
            }
        ],
        isCorrect,
        isAnswered = false

    const shuffle = (array) => {
        array.sort(() => Math.random() - 0.5)
    }

    const checkQuestion = (correct) => {
        isAnswered = true
        isCorrect = correct
    }

    shuffle(allAnswers)

</script>

<h3>
    {@html question.question}
</h3>
{#if isAnswered}
    <h4>
        {#if isCorrect }
            You got it right
        {:else}
            You goofed
        {/if}
    </h4>
{/if}

{#each allAnswers as answer}
    <button on:click={() => checkQuestion(answer.correct)}>
        {@html answer.answer}
    </button>
{/each}